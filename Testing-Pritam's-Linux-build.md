# Testing Pritam's Linux build

There has been a lot of interest in the Brackets Community around getting Brackets running on Linux. The 
(Linux Version thread in the brackets-dev forum)[https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/29vOJ6tvl8A[101-125-false]] got so large and hard to follow that I created this wiki page to summarize the info so far, and also as a collecting point for more info. The following info was copied from (Pritam Baral)[https://github.com/pritambaral].

With reasonable confidence I can say Brackets for Linux is ready for testing. Two builds (32 and 64 bit respectively) have been posted: (github.com/pritambaral/brackets-shell/downloads)[https://github.com/pritambaral/brackets-shell/downloads]. The core brackets code from Sprint 16 has also been packaged and posted on the same page. A tester may chose to build it for himself, or download the brackets-shell-[64,32] and brackets-sprint-16 builds and run it.

Both brackets-shell and brackets are packaged up as brackets-shell-[32,64]-bit.tar.bz2 and brackets-sprint-16.tar.bz2 respectively on the downloads page. Download both and extract to the proper directory. The folders www/ and samples/ being in the same folder as the Brackets binary is preferable, but you extract anywhere and manually choose the location of www/index.html on start-up.

_[Note from Randy Edmunds: it wouldn't work for me on OpenSuse 12.1 when I put these folders side-by-side, but it did work when I put the www/ and samples/ being in the same folder as the Brackets.]_

Or, if you want to test the building process, the linux branches on both (pritambaral/brackets)[https://github.com/pritambaral/brackets] and (pritambaral/brackets-shell)[https://github.com/pritambaral/brackets-shell] are the ones to use.


