Heroku buildpack: Python-ffmpeg
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/). The only difference from the clean Python buildpack is that this one includes ffmpeg. A static build is simply cloned from [this repo](https://github.com/integricho/ffmpeg1.2-static-with-codecs), which includes a couple of useful codecs (see the ffmpeg output below), but still trying to be light on disk space.

Usage
-----

Example usage:

    $ ls
    gunicorn.py.ini  Procfile  requirements.txt  wsgi.py
    $ git init
    Initialized empty Git repository in /path/to/app/.git/
    $ heroku create --stack cedar --buildpack git://github.com/integricho/heroku-buildpack-python-ffmpeg.git
    Creating random-app-name-1234... done, stack is cedar
    BUILDPACK_URL=git://github.com/integricho/heroku-buildpack-python-ffmpeg.git
    http://random-app-name-1234.herokuapp.com/ | git@heroku.com:random-app-name-1234.git
    Git remote heroku added
    $ git add .
    $ git commit -m "Initial commit."
    [master (root-commit) 16d444a] Initial commit.
     4 files changed, 29 insertions(+)
     create mode 100644 Procfile
     create mode 100644 gunicorn.py.ini
     create mode 100644 requirements.txt
     create mode 100644 wsgi.py
    $ git push heroku master
    Counting objects: 6, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (6/6), 786 bytes, done.
    Total 6 (delta 0), reused 0 (delta 0)

    -----> Fetching custom git buildpack... done
    -----> Python app detected
    -----> ffmpeg will be cloned into /app/.heroku/vendor/ffmpeg
    -----> Cloning pre-built ffmpeg repository...
    -----> No runtime.txt provided; assuming python-2.7.6.
    -----> Preparing Python runtime (python-2.7.6)
    -----> Installing Distribute (0.6.36)
    -----> Installing Pip (1.3.1)
    -----> Installing dependencies using Pip (1.3.1)
           Downloading/unpacking gevent==0.13.8 (from -r requirements.txt (line 1))
             Running setup.py egg_info for package gevent

           Downloading/unpacking gunicorn==0.17.2 (from -r requirements.txt (line 2))
             Running setup.py egg_info for package gunicorn

           Downloading/unpacking greenlet (from gevent==0.13.8->-r requirements.txt (line 1))
             Running setup.py egg_info for package greenlet

           Installing collected packages: gevent, gunicorn, greenlet
             Running setup.py install for gevent
               building 'gevent.core' extension
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/app/.heroku/python/include/python2.7 -c gevent/core.c -o build/temp.linux-x86_64-2.7/gevent/core.o
               gcc -pthread -shared build/temp.linux-x86_64-2.7/gevent/core.o -levent -o build/lib.linux-x86_64-2.7/gevent/core.so
               Linking /tmp/pip-build-u49411/gevent/build/lib.linux-x86_64-2.7/gevent/core.so to /tmp/pip-build-u49411/gevent/gevent/core.so

             Running setup.py install for gunicorn

               Installing gunicorn_paster script to /app/.heroku/python/bin
               Installing gunicorn script to /app/.heroku/python/bin
               Installing gunicorn_django script to /app/.heroku/python/bin
             Running setup.py install for greenlet
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -fno-tree-dominator-opts -I/app/.heroku/python/include/python2.7 -c /tmp/tmpvtXjyE/simple.c -o /tmp/tmpvtXjyE/tmp/tmpvtXjyE/simple.o
               /tmp/tmpvtXjyE/simple.c:1: warning: function declaration isn’t a prototype
               building 'greenlet' extension
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -fno-tree-dominator-opts -I/app/.heroku/python/include/python2.7 -c greenlet.c -o build/temp.linux-x86_64-2.7/greenlet.o
               In file included from slp_platformselect.h:12,
                                from greenlet.c:416:
               platform/switch_amd64_unix.h:40: warning: ‘noclone’ attribute directive ignored
               platform/switch_amd64_unix.h:43: warning: ‘noclone’ attribute directive ignored
               gcc -pthread -shared build/temp.linux-x86_64-2.7/greenlet.o -o build/lib.linux-x86_64-2.7/greenlet.so
               Linking /tmp/pip-build-u49411/greenlet/build/lib.linux-x86_64-2.7/greenlet.so to /tmp/pip-build-u49411/greenlet/greenlet.so

           Successfully installed gevent gunicorn greenlet
           Cleaning up...
    -----> Discovering process types
           Procfile declares types -> web

    -----> Compressing... done, 71.9MB
    -----> Launching... done, v5
           http://random-app-name-1234.herokuapp.com deployed to Heroku

    To git@heroku.com:random-app-name-1234.git
     * [new branch]      master -> master
    $ heroku run bash
    Running `bash` attached to terminal... up, run.1627
    ~ $ ffmpeg
    ffmpeg version 1.2-static Copyright (c) 2000-2013 the FFmpeg developers
      built on Jan 26 2014 09:28:15 with gcc 4.6 (Ubuntu/Linaro 4.6.3-1ubuntu5)
      configuration: --prefix=/home/andrean/dev/tools/ffmpeg-static/target --extra-cflags='-I/home/andrean/dev/tools/ffmpeg-static/target/include -static' --extra-ldflags='-L/home/andrean/dev/tools/ffmpeg-static/target/lib -lm -static' --extra-version=static --disable-debug --disable-shared --enable-static --extra-cflags=--static --disable-ffplay --disable-ffserver --disable-doc --enable-gpl --enable-pthreads --enable-postproc --enable-gray --enable-runtime-cpudetect --enable-libfaac --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libx264 --enable-libxvid --enable-bzlib --enable-zlib --enable-nonfree --enable-version3 --enable-libvpx --disable-devices
      libavutil      52. 18.100 / 52. 18.100
      libavcodec     54. 92.100 / 54. 92.100
      libavformat    54. 63.104 / 54. 63.104
      libavdevice    54.  3.103 / 54.  3.103
      libavfilter     3. 42.103 /  3. 42.103
      libswscale      2.  2.100 /  2.  2.100
      libswresample   0. 17.102 /  0. 17.102
      libpostproc    52.  2.100 / 52.  2.100
    Hyper fast Audio and Video encoder
    usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...

    Use -h to get full help or, even better, run 'man ffmpeg'
    ~ $ exit

You can also add it to upcoming builds of an existing application:

    $ heroku config:add BUILDPACK_URL=git://github.com/integricho/heroku-buildpack-python-ffmpeg.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root.

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug.

Specify a Runtime
-----------------

You can also provide arbitrary releases Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-3.3.2

Runtime options include:

- python-2.7.4
- python-3.3.2
- pypy-1.9 (experimental)
