## Introduction

Right now, we have little information about the experience people have with Brackets. How long does it take to start up? How quickly can they switch files? How about switching projects? Have they experienced crashes or hangs recently? Are there unnoticed error messages appearing on the dev tools console? This kind of information is generally good for us to know so that we can make sure that Brackets is improving from release-to-release.

Brackets has an extraordinarily powerful extension system and we rely on extensions to give everyone the Brackets that fits what they need to do. But those extensions inject even more variability into Brackets’ performance and stability. We get bug reports sometimes that we track down to an extension, but we have no way of knowing how widespread the problem is. Some people have 20 or more extensions installed, which is great! But it’s also a headache when it comes to making correlations with problems that people are seeing.

For many users, Brackets is an application they use all day long. Being able to tune Brackets to run well everywhere is important both to users of Brackets and to the project as a whole.

Because of all of this, we feel that it's important to gather useful, actionable information. As a fully open project, we only want to gather information in a way that is transparent and respectful to our users. I’ve looked at other open source projects, particularly the [Firefox Health Report](https://blog.mozilla.org/metrics/2012/09/21/firefox-health-report/) (FHR), as models.

We want to collect information such as (pulling some terms from the FHR):

* Configuration data (Brackets version, OS version, certain preferences that affect operation of Brackets)
* Customization data (extensions installed and enabled)
* Performance data (startup time, aggregate file switching time, project open time)
* “Wear and Tear” data (number of crashes, length of time that Brackets has been open, age of the cef_data directory)
* “Meeting user needs” data (how often are features like Live Preview and Inline Editors used? This is data similar to what websites get when you click on a link or button.)

We’ll provide UI to make the data we collect visible to you and hopefully useful to you in seeing how well your Brackets is running. We'll also present the aggregate data so that everyone has a feel for how the project is doing.

The data will be anonymized and turning off data collection will be easy and the first run will make it easy to turn off right away if you’re not interested in helping the project in this way. Finally, Brackets is open source which means that the client-side code will be fully open. There will be no mysteries around what we collect.

## Initial Data Proposal

These are the pieces of information we look to collect in the first iteration of the feature.

* UUID
* Snapshot time (UTC)
* Brackets Version
* OS
* OS version
* List of installed extensions *that are available in the registry* and their versions

## Next Up Data Proposal

We wish to collect this information soon. "Active time" increases as the user is typing/running commands in Brackets and stops increasing after an idle period.

* "Active time" using Brackets
* "Active time" per file type (file extension)
* Command counts for a specific list of commands
    * navigate.toggleQuickEdit
    * navigate.toggleQuickDocs
    * file.liveFilePreview

## How the Data is Stored and Sent

The initial data proposal consists of "snapshot" data which is collected when the data will be sent.

There are two timestamps saved in viewstate: time of last snapshot and time of last successful transmission

* A single pref will turn off all of this activity
* When Brackets starts, see if it has been at least 24 hours since the last data snapshot or successful transmission
* If not, set a timer based on the snapshot time
* When the time is right for a snapshot
    * Gather the data
    * Store it in a JSON file in a subdirectory of app data.
    * Update the snapshot time in viewstate
    * Attempt to send all unsent data to the collection server
    * If the data is accepted, store the last transmission time in viewstate.
    * If the data is not accepted (problems contacting server or other server errors), set a timer to try sending again (a pseudo-random amount of time) later

## UI

* A first run display will explain the collection and provide buttons to accept collection or view a sample of the data
* The sample is an actual snapshot of the user's info. It is not sent. The user can turn off collection from that view.
* There will be a menu item to show health data.
* The initial version will show the current snapshot and will also provide a button to stop collection (or restart it)
* A button will be provided in that UI to open the directory that contains the snapshots

## The Temporary Server

A simple log collector for streaming JSON data.

* Malformed input is logged as such but does not result in an error condition (so a client that is sending odd data for some reason will keep trying to send data)

Basic process:

* Collect up a day's worth of logs, moves on to next log file
* server gzips that day's data
* the data is sent up to s3 for archiving

## Reporting?

This is a proposal for a temporary collection system and right now there's no plan for built-in reporting. Reporting will be done by writing scripts against the data.