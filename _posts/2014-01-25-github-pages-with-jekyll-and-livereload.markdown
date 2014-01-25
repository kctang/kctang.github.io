---
layout: post
title:  "Creating GitHub Pages with Jekyll & LiveReload"
date:   2014-01-25 09:28:00
categories: jekyll livereload
---

Composing blog posts in Markdown is great but it would be even better if I can see how pages are rendered live. So, let's have that.

First thing, get a Jekyll blog set up. create a new blog structure in the current directory with:

    jekyll new .

See [quick start guide][quick-start] for details on viewing it from Jekyll's built-in server.

Next, enable live reload as you are editing content. Basically, it works as a Grunt project that uses [grunt-contrib-watch][grunt-contrib-watch] plugin's support for livereload. It also bypasses Jekyll's built-in web server and use Grunt's web server for hosting - not a big deal but just so you know.

## Create/Update Configuration Files

Create `Gruntfile.js`:

    module.exports = function (grunt) {
        grunt.initConfig({
            shell: {
                jekyllBuild: {
                    command: 'jekyll build'
                }
            },
            connect: {
                server: {
                    options: {
                        port: 8080,
                        base: '_site'
                    }
                }
            },
            watch: {
              livereload: {
                files: [
                    '_config.yml',
                    'index.html',
                    '_layouts/**',
                    '_posts/**',
                    '_includes/**',
                ],
                tasks: ['shell:jekyllBuild'],
                options: {
                  livereload: true
                },
              },
            }
        });

        grunt.loadNpmTasks('grunt-contrib-connect');
        grunt.loadNpmTasks('grunt-contrib-watch');
        grunt.loadNpmTasks('grunt-shell');
        grunt.registerTask('default', ['shell', 'connect', 'watch'])
    }

Create `package.json`:

    {
      "name": "My Jeykll Blog",
      "version": "0.0.1",
      "devDependencies": {
        "grunt": "~0.4.2",
        "grunt-contrib-connect": "~0.6.0",
        "grunt-contrib-watch": "~0.5.3",
        "grunt-shell": "~0.6.4"
      }
    }

Update `_config.yml` to exclude these non-Jekyll files from being processed by Jekyll:

    exclude: [node_modules, Gruntfile.js, package.json]

Update `.gitignore` to exclude node_modules from being committed.

## Pre-Requisites
To make this work, there are some pre-requisites.

* Install [LiveReload][live-reload] plugin for Chrome browser.
* Install npm. FWIW, [node-and-npm-in-30-seconds.sh][install-npm] might be useful if you are not familiar with npm.

## Running
Type `npm install` to download the required Grunt dependencies for the first time.

After that, just type `grunt` to start a web server at port 8080 to host your blog with live reload enabled.

    → grunt
    Running "shell:jekyllBuild" (shell) task

    Running "connect:server" (connect) task
    Started connect web server on http://localhost:8080

    Running "watch" task
    Waiting...

Hope you find this helpful.

[quick-start]: http://jekyllrb.com/docs/quickstart/
[live-reload]: https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei
[grunt-contrib-watch]: https://github.com/gruntjs/grunt-contrib-watch
[install-npm]: https://gist.github.com/isaacs/579814