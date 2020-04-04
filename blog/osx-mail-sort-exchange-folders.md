Title: Get OS X Mail to sort Exchange Folders Alphabetically
Date: 2016-11-14 09:30
Category: How To
Tags: osx, mail, exchange
Authors: Shane Maloney
Summary: Sort Exchange folder alphabetically in OS X Mail

Original source [here](https://discussions.apple.com/message/11157240#11157240)

Newly created folders are not sorted alphabetically to sort this out:

1. Quit Mail app
1. Run the following command in a terminal 
`cp /Users/<username>/Library/Mail/V3/<mailbox>/.mboxCache.plist{,.bck}`
1. Restart Mail check if folder are displaying correctly and all working.
1. Remove `.bck` file
 
