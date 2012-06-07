--- 
layout: post
title: Python Script For Automated Voting
published: true
meta: 
  _edit_last: "2"
  _wp_old_slug: "6"
  DiggUrl: http://digg.com/programming/Python_Script_For_Automated_Voting
tags: 
- automated voting
- automation
- Code
- python
type: post
status: publish
---
This was a python script created by me a while ago to automate voting for a particular user.  It may not work anymore, as this script was written quite a while ago.  It's not commented or anything. Here is the original post:

I created a small script to do the clearing of the cookies for you and everything. I tried to have it solve the captcha for you automagically, but it just wasn't happening (I used ocrad and tesseract, and cleaning the image using imagemagick first). Anyways, you can sit there all day and enter the captchas to keep voting.

A few requirements:
* I wrote it using python 2.5.1. Not sure what other version it will work on.
* PIL required
* mechanize required

both of the required modules are available in the ubuntu repositories. I think that's all you need, let me know if you run into any troubles.
<pre lang="python">sudo apt-get install python-imaging python-mechanize</pre>
A couple notes about it:
* You don't have to hit "submit", you can just press enter
* check the console for updates on whether it was submitted ok or not

Let me know if you want help learning how to set it up.
<pre lang="python">#!/usr/bin/python

# Displays the Captcha from http://basketball.seniorclassaward.com/public/men/vote.aspx?usr=public&amp;gen=M
# so that you can enter it so that Jacee Carroll will win. It automagically keeps track/clears cookies (via mechanize)
# so you can submit more than once every 24 hours

# Written by Josh

import pygtk, gtk
import StringIO
import ImageFile
import re
from mechanize import Browser
pygtk.require('2.0')

# taken from http://www.daa.com.au/pipermail/pygtk/2003-June/005268.html
def image_to_gtkpixbuf(image):
    file = StringIO.StringIO ()
    image.save (file, 'ppm')
    contents = file.getvalue()
    file.close ()
    loader = gtk.gdk.PixbufLoader ('pnm')
    loader.write (contents, len (contents))
    pixbuf = loader.get_pixbuf ()
    loader.close ()
    return pixbuf

class Submitter:

    def get_image(self):
        # Get the image
        image_url = 'http://basketball.seniorclassaward.com/Captcha.aspx'

        image_response = self.br.open_novisit(image_url)

        p = ImageFile.Parser()
        while 1:
            data = image_response.read(1024)
            if not data:
                break
            p.feed(data)
        image_response.close()
        return p.image

    def __init__(self):
        self.br = Browser()
        self.br.set_handle_robots(False)
        self.url = 'http://basketball.seniorclassaward.com/public/men/vote.aspx?usr=public&amp;gen=M'
        self.br.open(self.url)

        self.captcha = self.get_image()

    def submit(self, captcha_text):
        captcha_textbox = 'ctl00$DefaultContentPlaceholder$PublicBallot$CaptchaTextBox'
        jaycee_checkbox = 'ctl00$DefaultContentPlaceholder$PublicBallot$BallotCheckBoxList$0'

        br = self.br
        br.select_form( name='aspnetForm' )
        br[captcha_textbox] = captcha_text
        br[jaycee_checkbox] = ['on']

        response = br.submit()

        r = response.read()
        if re.search(r"ATTENTION:\\n([^']+)'", r):
            res = re.search(r"ATTENTION:\\n([^']+)'", r)
            print "Error submitting: " + res.groups()[0]
            return False
        elif re.search(r"COMPLETE:\\n([^']+)'", r):
            res = re.search(r"COMPLETE:\\n([^']+)'", r)
            print "SUCCESS! " + res.groups()[0]

        return True

class Control:
    def __init__(self):
        self.window = gtk.Window()
        self.window.connect("destroy", gtk.main_quit)

        self.submitter = Submitter()

        self.box = gtk.HBox(False, 0)
        self.window.add(self.box)

        self.submitButton = gtk.Button("Submit")
        self.submitButton.connect('clicked', self.submit)

        self.box.pack_start(self.submitButton, True, True, 0)
        self.submitButton.show()

        self.text = gtk.Entry()
        self.text.connect("activate", self.submit)
        self.box.pack_start(self.text, True, True, 0)
        self.text.show()

        self.set_image(self.submitter.captcha)
        self.box.pack_start(self.image, True, True, 0)
        self.image.show()

        self.box.show()
        self.window.show()

        self.text.grab_focus()

    def set_image(self, image):
        self.image = gtk.Image()
        self.image.set_from_pixbuf(
                image_to_gtkpixbuf(image))

    def change_image(self):
        self.box.remove( self.image )
        self.set_image( self.submitter.get_image() )
        self.box.pack_start(self.image, True, True, 0)
        self.image.show()
        return True

    def submit(self, evt):
        if self.submitter.submit( self.text.get_text().upper() ):
            # reset the submitter
            self.submitter = Submitter()
            self.change_image()

            self.text.set_text('')
            self.text.grab_focus()

def main():
    gtk.main()
    return 0

if __name__ == "__main__":
    Control()
    main()</pre>
This was a just-for-fun thing for me, so I could test out some various web tools for python. I tried simple urllib/urllib2, ClientForm, ClientCookies, and mechanize, and found mechanize to be the most robust and easiest to use, although it was missing out on some features that I thought may be useful (as higher-level modules normally do).

Also, I didn't like this:
<pre lang="python">        if re.search(r"ATTENTION:\\n([^']+)'", r):
            res = re.search(r"ATTENTION:\\n([^']+)'", r)</pre>
