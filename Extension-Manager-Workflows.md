This document proposes a set of user and extension developer workflow scenarios that we want 
the Brackets Extension Manager to support.

> (kd) General question: do we want to add social sharing (so that users can share an interesting extension on Twitter, Facebook, G+)?

> (nj) Seems like a second-tier feature. I'd like to keep this workflow to (mostly) first-tier
> features for now--I don't feel like adding sharing will significantly affect the overall UI design
> (it would probably just be another button associated with the extension, perhaps in the "More..."
> section).

## Discovery

John, a Brackets user, is starting to experiment with CoffeeScript. He opens a CoffeeScript
file and sees that it's color-coded, but there doesn't seem to be any other built-in support
for it. He wonders if there are Brackets extensions available that add CoffeeScript-specific
features.

In the toolbar on the right-hand side of the Brackets window, the topmost icon is the
Extension Manager. He clicks on the icon, and a modal Extension Manager dialog appears.

> Note that I'm deliberately saying that the Extension Manager icon deserves to be
> the first icon on the toolbar (and I'm assuming the Topcoat layout here, with the
> vertical toolbar on the right). Of course, we should have an ordinary menu item
> as well, though it's not obvious where it should go; Chrome puts it in the Window
> menu, which seems a little odd, while Firefox puts it in the Tools menu (which we
> don't have). On the Mac, you could also argue for the application menu, but that
> doesn't exist on Windows.

At the top of the Extension Manager are two tabs, "Find" and "Manage", with the
"Find" tab preselected, and a button to the right labelled "Create". Within the 
Find tab is a search field, and below the field is a list of popular extensions, 
sorted by rating. He briefly browses this and notes that there's a cool-sounding 
extension for GitHub integration that's popular; he clicks the Install button on
it, and it starts installing in the background.

> Note that this implies that extension download and installation is asynchronous and
> non-modal as well as restartless. 

While it's downloading and installing, he continues his search for CoffeeScript 
extensions. He types "coffee" into the search field. As he types, the list of 
popular extensions in the main part of the window is filtered to show a list 
of all extensions that match his typed search, also sorted by rating.

> (kd) is it limited to just the popular extensions, or will it put the popular/highly rated on top
> and seek out all of the coffee extensions?

> (nj) It will show all matching extensions, but just sort them by popularity. We
> could add a way to change the sort order (e.g. by Quick Open match heuristic or
> alphabetically).

He sees that there are three extensions that sound interesting at the top of the list: 
Auto-Compile CoffeeScript, CoffeeScript Quick Open/Quick Edit, and CoffeeScript
Formatting. Each extension has a screenshot thumbnail, a title, the first few lines 
of a description (with a "More..." link), and an "Install" button on the right.

> (kd) if there are screenshots, are any displayed in thumbnail form in this view?

> (nj) That makes sense, but I actually wonder how meaningful a small thumbnail of a
> screenshot would be--they might all kind of look the same. Still, I'll put it in
> here and we can see how it works out.

He clicks on the "More..." link on the first extension to see more info. It shows
the full screenshot and more of the description text, which explains the basic usage 
of the extension, along with a link to the extension developer's website for more info.

## Installation

He doesn't feel like he needs to read any more for the other extensions, so he clicks 
on each of the "Install" buttons on these three extensions. Each button changes to a 
grayed-out "Installing..." button.

A progress bar appears at the bottom of the window showing the download and install
progress for all three extensions, starting with "Installing Auto-Compile CoffeeScript
(1 of 3)...". As they complete, a checkmark appears next to each extension indicating 
that it's been installed, and the "Install" button on each extension is replaced by 
two buttons, "Disable" and "Uninstall".

> (kd) has there been a change in which tab he's on to see disable/uninstall?
> Those would be in the Manage tab, right?

> (nj) Good point. I guess I was assuming that the view of a given extension would be
> the same no matter which tab you're in (and I don't think it would make sense to
> *eliminate* installed extensions from the "Find" view). But then that suggests that the 
> "Manage" tab isn't really necessary, in a way&mdash;the list of installed extensions 
> could just be another filter on the list of all extensions. Would it make sense to just
> have a single view with the search box, and then an "All" vs. "Installed" radiobutton?
> Or is it clearer to have separate tabs?

Also, next to the "CoffeeScript Formatting" extension, there is a "Settings" button.
He clicks on this button, and a dialog expands in-place with various formatting
settings for CoffeeScript code. He tweaks it to fit his style. Finally, he clicks
the Close button on the Extension Manager dialog to close it.

Now, back in his CoffeeScript file, he tries hitting Cmd-E on a function call, and
an inline editor opens on the function definition. Success! He also browses the
menus to see what other functionality has been added by his new extensions.

## Feedback

As he starts using the extensions, he notices that they mostly work well, but the
CoffeeScript Formatting extension doesn't seem to work properly&mdash;when he tries to
use it, his code gets mangled. He opens the Extension Manager and clicks on the
"Installed" tab, which shows all of his installed extensions. He clicks "Disable"
next to the CoffeeScript Formatting extension. He also sees that there's a "Report
a Problem" dropdown button, along with a rating widget. 

He wants to give the extension developer the benefit of the doubt, so rather than 
rating it, he clicks "Report a Problem". A dropdown menu appears with three choices:
"Report a Bug", "Report Version Incompatibility", and "Report Abuse". He chooses
"Report a Bug", and is taken to the developer's GitHub issue tracker.

He's been really impressed with the Quick Open/Quick Edit extension, so he clicks
on the rating widget next to that one to give it a 5. Brackets prompts him to
login using OpenID in order to rate it. He does so, and chooses to let Brackets
remember his credentials.

> We probably need some sort of login for ratings to avoid abuse. I'm assuming
> OpenID makes the most sense, but Facebook, Twitter, G+, etc. logins might be
> more likely to be convenient.

> (kd) This is going to require a little thought about the authentication flow,
> since we're not in the browser. If we open up an iframe to Twitter within Brackets,
> for example, the user would have to actually authenticate. I wonder if it's possible
> to open the user's browser, have them approve the oauth request there, and then
> return them to Brackets where they're logged in.
> After writing that, it seems a bit worse to do it that way than to have them type
> in their Twitter username/password in a Brackets-controlled window.

> (nj) Yeah, I feel like I've seen apps that pop up their own Facebook, Twitter, etc.
> windows, authenticating within that app window, and then caching the credentials
> in that app (though I don't even exactly know how the credential stuff works--I'm
> assuming there's a token that the app hangs onto that expires after some time).
> Since we *are* a browser (but not in the user's browser), I'm assuming that we
> could just do this the same way a normal web app does.

> Also, I'm not sure how credential remembering would work&mdash;would we need to
> have it time out after awhile?

> (kd) seems like a good question for Andrew. For the purposes of rating/reviewing things,
> I'm not so worried. For publishing extensions, it's more of a concern.

> (nj) If things work the way I mentioned above, then presumably we could just hold
> onto the auth token until it expires (which is presumably controlled by whichever
> sign-in service provided the token).

## Update

The next day, John sits back down at his computer and brings Brackets to the
front. He sees that there's a little "1" badge on the Extension Manager icon.
He clicks on the Extension Manager, and a notification at the top informs him
that an update is available to the CoffeeScript Formatting extension. The
update descriptions describes the bugs that were fixed in the update--one of
which was his bug! He clicks on the "Update" link in the notification, and the 
update is downloaded and installed. Brackets prompts him if he wants to re-enable 
the extension now that it's been updated, and he clicks Yes.

Closing the Extension Manager, he tries the formatting command again, and
lo and behold, it works! He goes back to the extension developer's bug
tracker and lets him know the bug was fixed, and gives the command a rating
for good measure.

## Developing a new extension

While he's working on his CoffeeScript file, John is curious about the JS
that's being generated, and realizes that it would be cool to have a Brackets
extension that shows the generated JS for the current function as you type.
He figures he can write it himself.

He remembers that there was a "Create" button in the Extension Manager, so
he opens it up and hits "Create". A dialog appears prompting him for the
name of the extension, and showing that it will be created in his "dev"
folder. He enters "CoffeeScript Preview" as the name and clicks OK. Brackets
creates the extension project in his "dev" folder and switches to it, and
he sees in the file tree that a main.js, a package.json, a README, and other 
boilerplate files have already been created for his extension. 

He starts writing and debugging the extension, and loves how easy it is to 
get it working.

As he's working on the extension, he realizes that he wants to check it into
GitHub. He remembers the GitHub extension he saw in the Extension Manager, and
goes and installs it, so he can create the repo and check in the extension
from Brackets.

## Uploading

When he's happy with the extension, he wants to share it with others. He goes
back into the Extension Manager and clicks on the "Manage" tab. Inside that
tab, along with the other extensions he'd installed, he sees his own extension
with a special "unpackaged" icon, and an "Upload" button next to it. He clicks
on the "Upload" button, and the extension is packaged and uploaded to the
Brackets registry, using the metadata from his package.json file, and registering
it using the OpenID he had logged in with earlier. The icon changes to indicate 
that he is still using his own development version of a registered extension.

He clicks "Disable" on his development version, and then chooses to install
the "official" version he just uploaded to test it out. It seems to be working
fine, so he disables the official version and re-enables his development version
so he can keep hacking on it.

> This is a little funny&mdash;not sure if this is the right way to handle this
> scenario or if there's something smarter we want to do here.

> (kd) I guess we could handle this case specifically. In addition to the "Upload"
> button there could be a "Try Published Version" button that acts as a toggle
> between the released version and the in-development version.