# CouchDB 3.3.1 for Mac is Borked.

But don't despair. It's a [known issue](https://github.com/apache/couchdb/issues/4372).

But maybe despair, because it isn't fixed yet. And the solution requires homebrew...which can be a can of worms.

Here's what worked for me on March 8, 2023.

##### Disclaimer!
You could potentially break something if you've ever used homebrew to install something. Run `brew list` in your terminal. If a bunch of stuff pops up, it means you have a current `homebrew` install, and that you've used it to install things. Don't nuke it if you are uncertain!

##### How I Got CouchDB 3.3.1 Working:
1. Uninstall everything related to homebrew.
2. Uninstall everything related to CouchDB.
3. Uninstall xcode Command Line Tools.
4. REBOOT
5. Install [Command Line Tools for Xcode 14.2](https://developer.apple.com/download/all/?q=command%20line).
6. Install [homebrew](https://brew.sh/).
7. Now that you have homebrew, use it to run `brew install spidermonkey@91`
8. Download the [CouchDB 3.3.1 .zip file](https://couchdb.apache.org/).
9. Unzip it and move it to your Applications folder.
10. REBOOT
11. Open CouchDB. It works?!

I'm not sure what mattered in this 👆 process, but this series of events worked. I wonder if it might've been important to have `spidermonkey@91` installed BEFORE opening CouchDB for the first time? 🤷‍♂️