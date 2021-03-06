### 2017-09-27-Wednesday

- The idea: WordPress as content-managment-system for static pages. Stories created/edited in wordpress could be ingested into the new iip stories site.
    - Some benefits to this, over markdown...
        - Wordpress offers friendlier visual-editing tools.
        - It offers more formatting options.
        - It versions each page's content; it's easy to compare versions and revert.
        - It offers Brown and non-Brown logins.
        - The Library has its own wordpress install, which is heavily used and not going away.

- [my example test site](https://worfdev.services.brown.edu/create/birkin/)
    - this server requires vpn
    - see the different pages, like [this one with images](https://worfdev.services.brown.edu/create/birkin/this-is-a-new-page/)
    - i'll give each of you editor permissions on this experimental site, so you can freely edit these pages.

- [wordpress api info](https://codex.wordpress.org/WordPress_APIs)
    - in particular the [rest-api](https://developer.wordpress.org/rest-api/)
    - there's a [demo external api site for experimenting](https://demo.wp-api.org)
    - from a hopeful experiment, it looks like you can use the rest-api example calls on any wordpress site!
        - so, for my site, the [pages api](https://developer.wordpress.org/rest-api/reference/pages/) that lists pages yields [this json](https://worfdev.services.brown.edu/create/birkin/wp-json/wp/v2/pages). Cool, eh?
        - from that page-listing output, you can see that the `this-is-a-new-page` webpage, referenced above, has an id of 5, so you can use the single-page pages-api to get that page's content, [like this](https://worfdev.services.brown.edu/create/birkin/wp-json/wp/v2/pages/5)

- an issue
    - I'm not clear how to get access to, or utilize the css that's associated with a page, but I think there are multiple options related to this.

---

### 2017-08-07-Monday

- showed W. & M. that adding a `<div classorid="foo">` tag in the markdown might be one solution for more customized css for stories.
    - idea assumes that the Stories model/table _might_ include an optional custom-css path or url.
    - W. suggested good idea of having a few pre-set css classes that could just be dropped in to the markdown.

- showed W. & M. that the existing admin-markdown implementation is based on an installed library: [django-markdown-deux](https://github.com/trentm/django-markdown-deux), which itself uses the library [python-markdown2](https://github.com/trentm/python-markdown2) -- which offers possibly-useful features.
    - the python-markdown2 library [readme](https://github.com/trentm/python-markdown2#extra-syntax-aka-extensions) shows that features beyond standard-markdown can be enabled -- like footnotes -- with a link to these '[extras](https://github.com/trentm/python-markdown2/wiki/Extras)' documentation.
    - the django-markdown-deux library installed into django implements these 'extras' in the [django settings file](https://github.com/trentm/django-markdown-deux#markdown_deux_styles-setting).
        - the default summer-student project [specifies a minimal set of extras](https://github.com/Brown-University-Library/iip_smr_web_project/blob/f75a6d8aef2252bc783a035addf5a147ce7a332c/iip_smr_config/settings.py#L114-L124); it may be worth experimenting with what else could be enabled.



### 2017-07-31-Monday

- showed M. how the markdown-admin works for static pages.
    - student thought that, given G's presentation this week, and given high initiation-cost, wouldn't be good to do much admin/markdown work this week

- followup thought to communicate to W. re branch vs master changes...
    - Situation: G. presents this week, so we want to minimize chance of unexpected problems with the site
    - So question: can they commit development to master, and just not run the script to update the server?
    - I recommended using a branch, but noted the commit-to-master without updating the server could be an alternative.
        - with further thought, I more strongly recommend a branch for the followig scenario: Imagine a last-minute bug is found that really needs to be fixed. It would be _so_ much easier to fix the bug if updating the server's master branch did _not_ introduce lots of other changes.


### 2017-07-27-Thursday

- unicode & python info...

    - first, unicode-strings and byte-strings...

        I think of a unicode representation of a letter as some sort of abstract perfect theoretical representation of that letter.

        How that letter gets 'written' to a file on the disk -- how it is 'encoded' -- can happen different ways.

    - python2

            >>>
            >>> unicode_string = u'föo'
            >>>
            >>> type( unicode_string )
            <type 'unicode'>
            >>>
            >>> unicode_string.encode( 'utf-8' )
            'f\xc3\xb6o'
            >>>
            >>> unicode_string.encode( 'latin-1' )
            'f\xf6o'
            >>>

            >>>
            >>> byte_string = unicode_string.encode( 'utf-8')
            >>>
            >>> type( byte_string )
            <type 'str'>
            >>>
            >>> ### _always_ use 'utf-8' to encode ###
            >>>

    - python3

            >>>
            >>> unicode_string = 'föo'  # no need for the preceeding 'u' -- in python3 all strings are unicode by default
            >>>
            >>> type( unicode_string )
            <class 'str'>
            >>>
            >>> # the above can be _confusing_: 'str' in python3 means it's a unicode string; 'str' in python2 means it's a byte-string
            >>>
            >>> unicode_string.encode( 'utf-8' )
            b'f\xc3\xb6o'
            >>>
            >>> # that preceeding 'b' is the indicator that the string is a byte-string
            >>>
            >>> unicode_string.encode( 'latin-1' )
            b'f\xf6o'
            >>>

            >>>
            >>> byte_string = unicode_string.encode( 'utf-8')
            >>>
            >>> type( byte_string )
            <class 'bytes'>
            >>>
            >>> ### again, _always_ use 'utf-8' to encode ###
            >>>

    - main rule:
        - whenever you're working in code: _always_ make sure the string you're working with is a unicode string
            - sometimes this will happen automatically, sometimes not

    - practicality of the main rule:
        - data in a file will be in some type of encoding (meaning it's in byte-string format)
            - if you have _any_ control over the production of that data, make _sure_ it's encoded in utf-8
        - when you 'read' the data into your code, make __sure__ that __as soon as possible__ in your code, that the data has been converted from a byte-string to a unicode-string
        - keep it in unicode-format until you're ready to write it out

- laptop permissions fix

    - step a: update a gitconfig entry

            cd /path/to/iip_summer_dev_stuff/iip_smr_web_project
            git config core.sharedRepository group

    - step b: update project-directory permissions

            sudo chmod u=rwx,g=rwx,o=rx /path/to/iip_summer_dev_stuff/iip_smr_web_project
            cd /path/to/iip_summer_dev_stuff/iip_smr_web_project
            sudo find ./ -type d | xargs chmod u=rwx,g=rwx,o=rx
            sudo find ./ -type f | xargs sudo chmod u=rw,g=rw,o=r

- goals:

    - go over update script
        - cover concept of 'static files', which they don't have to think about on their own laptops, but do re the server

    - go throuch checking out master

    - git commands...
        - show all branches: `git branch -a`
        - switch to new branch: `git checkout name-of-branch`
        - delete branch: `git branch -d name-of-branch`
        - update server's knowledge of github current branches: `git fetch -p`

    - followup on past xml/xslt conversation, showing them [transformer url](https://library.brown.edu/xsl_transformer/v1/shib/?xml_url=https://library.brown.edu/bjd/transform_test/people.xml&xsl_url=https://library.brown.edu/bjd/transform_test/people_to_web.xsl&auth_key=shib
)
    - issues: have S. add an issue re the bib problem

    - github tips for referring to code
        - note ability to reference individual lines, or multiple lines, in github urls
        - note how to reference unchanging version of code

---


### 2017-07-26-Wednesday

- showed them the [working site](http://dcdscit.services.brown.edu/iip_smr_dev/search/) on the server
    - requires VPN

- covered automatic log-rotataion
    - mac: `cd /etc/newsyslog.d/`
    - showed sample format for file:

            $
            $ cat ./project_log.conf

            # idea: <http://randomerrata.com/post/64121939537/newsyslog-d>
            # docs: <http://www.freebsd.org/cgi/man.cgi?newsyslog.conf(5)>

            # logfilename                  [owner:group]  mode   count  size(KB)  when  flags [/pid_file] [sig_num]
            /path/to/project_logs/*.log    _www:staff     664    5      250       *     G

            $

- goal: master always works, even if not complete

- reminded them to check with G. re her upcoming presentation dates -- if they want to check out a branch on their server

- showed github [releases](https://github.com/Brown-University-Library/iip_smr_web_project/releases)
    - noted one approach: to make releases reflect versions of working code put into production
    - noted benefit: easy to find working version to revert to if things go south

- troubleshot template-error w/S.
    - briefly reviewed concept of reverse url-lookup
    - got the django 1.11 reverse url-lookup syntax working

- got student-update-script working; emailed S. & W. to try it; troubleshot S. problem report w/J.M. -- think we got it working

---


### undated stuff covered over about 3 meetings

- helped them install the updated version of C.R.'s code, [now using python3 & django-1.11](https://github.com/Brown-University-Library/iip_smr_web_project) onto their laptops

- made sure each could make a commit to the github repo

- noted virtual-environment 'gotcha' that deactivating a venv, or reloading, or loading a new one does _not_ eliminate environmental-variables from the environment

- gave them overview of indexing process:
    - inscription-editor makes changes to an inscription
    - editor commits change to github iip-texts repo
    - github fires off a 'webhook' notification to a web-app 'listener' I created
    - the web-app-listener:
        - initiates a git-pull for the iip-texts directory
        - determines which files have been added/updated, and which have been deleted
        - creates a solr-doc and updates solr for each addition/update
        - deletes from solr any needed deletions

- gave them overview of xml & stylesheets

- discussed approaches to 'Stories' feature S. would like to implement
    - showed them architecture of how basic markdown drives the '[welcome](http://dcdscit.services.brown.edu/iip_smr_dev/info/welcome/)', 'about', etc. pages in site
    - showed them how they can use that model to experiment with 'Stories' if they desire

- looked into S.'s report of a problem with bibliographies

---


### 2017-06-27-Tuesday

- goal: deploy to 'production' regularly
    - production being your dev server
    - meaning you should be able to email E.M., or Prof, a url every day of a new feature, or of added functionality

- related to the above, the utility of using a script to update the server code from github
    - the script does a git-pull, and...
    - 'touches' the file that makes the new code active
    - optional: the script can also update permissions on the code

- keep out of git:
    - passwords
    - server-paths
    - db and db-table names
    - strong suggestion: do not count on .gitignore to keep out settings
