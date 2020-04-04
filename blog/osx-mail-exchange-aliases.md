Title: Add aliases to OS X Mail Exchange Accounts
Date: 2010-12-03 10:20
Category: How To
Tags: osx, mail, exchange
Authors: Shane Maloney
Summary: Fixing issue IDL 8.5 issue with XQuartz 2.7

Issue can not add alias to Exhange accouns in OS X Mail (9.3)

Originally take from [here](https://codeisgreen.net/blog/2015/10/03/el-capitan-and-exchange-aliases-solution/)

Solution is to change outgoing mail server to none

1. In OSX Mail, select Mail -> Preferences.
1. Highlight your Exchange account from the list on the left.
1. Make sure the Account Information tab is selected in the right panel and look for the Alias dropdown.
1. The alias dropdown may be greyed out (it was for me) so, if it is, go to the Outgoing Mail Server dropdown and change it to none.
1. Close down that panel (or select another mail account from the left menu if you have one) to force it to save. It should ask you if you want to save settings.
1. Go back into that panel and you should find the Alias dropdown is no longer greyed out, so click the dropdown box and then select Edit Aliases.
1. You can now use the + and – buttons to add any aliases you want.
8. When you’ve added all your aliases, before you exit, change the Outgoing Mail Server entry back to Exchange.

That’s it. You shouldn’t even need to reboot the mail app. Your aliases should now be available to select via the ‘From‘ dropdown when you create or reply to a mail message.


