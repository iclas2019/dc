documents
=========

Documents app for Nextcloud

A Nextcloud app to work with office documents alone and/or collaboratively. It can open ODF documents and supports the basic features around text markup and inserting images. Tables can be displayed and edited but not created. Most other features are not supported. As the Documents app does not import documents to an internal representation and export them upon saving, editing will rarely damage documents, even if it can't display or handle elements.


Deprectated 💀
-------------

Sadly, WebODF is no longer developed and the company behind it has closed up. We might continue to try to keep Documents compatible with Nextcloud releases. But, as browsers change and office document formats evolve, the WebODF engine itself is  expected to start exhibiting more and more issues over time and it will cease to function at some point. Unless a significant time effort can be invested in WebODF, there is not much that can be done about this.

### Known issues ###
**Problem**: Editor doesn't open. Spinner spins for ages.  
**Solution**: Try to disable gzip for Documents app by adding the following line to your .htaccess:  
`SetEnvIf Request_URI .*/apps/documents/ajax/.* no-gzip dont-vary`

**Problem**: Doc(x) support doesn't work  
**Solution**: Install at least  `libreoffice-common` and  `libreoffice-writer` packages for your distro.  
If you don't want to mess around with dependencies, you need simply install `unoconv` package. It will do the trick for you.

### How to add more fonts ###
+ Upload font files to **documents/css/fonts** directory
+ Edit **documents/css/fonts.css** adding `@font-face` rule for each uploaded file 

### WebODF upgrade ###
1. Build WebODF:


        ./src/updateWebODF.sh prepare
or


        git clone https://github.com/kogmbh/WebODF.git webodf
        mkdir build
        cd build
        cmake ../webodf
        make all webodf-debug.js-target build-wodocollabtexteditor

2. Refresh code and create a new branch:

        cd /path/to/documents
        git checkout master
        git pull --rebase
        git checkout -b new-branch

3. Run upgrade script:

        ./src/updateWebODF.sh copy
        ./src/updateWebODF.sh patch

4. Resolve confilcts in patches (if any). Commit changes.
5. Update patches in `/path/to/documents/src/patches` according to conflicts. Commit changes.
6. Test UI. Fix glitches by updating CSS. Commit changes.
7. Run locale extraction script:

        ./src/updateWebODF.sh

8. Commit changes
9. Push the branch for testing
10. You are done.
