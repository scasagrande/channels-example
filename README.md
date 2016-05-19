Channels "Tutorial"
===================

There is no actual tutorial here, its just what I ended up writing after following the tutorial found on [Alberto Connor's blog](http://albertoconnor.ca/blog/2016/May/18/django-channels-background-tasks). You should check it out, its pretty cool.

What you need to do to get this running:

- Install your requirements

```bash
$ pip install -r requirements.txt
```

- In one terminal window, have `redis` running:

```bash
$ sudo apt-get install redis-server
$ redis-server
```

- In another terminal window, start the django server

```bash
$ python manage.py runserver
```

- This will start up a number of "workers" which will listen on your channels. On this particular machine as I write this, I get 4 workers spawned.
- While watching your browser tab and the terminal window, browse to [http://localhost:8000/channels_app/](http://localhost:8000/channels_app/)
- You should immediately see some dummy text in the browser window (`DERP TO THE MAX`) and the text `Hello, Channels!` in the terminal.
- Then, 10 seconds later, you should see "HI THERE" print in the terminal window.

In this demo, what happened was that there was an async call to `time.sleep(10)` and `print()`. The page rendered in a non-blocking fashion, while the print and sleep calls happened in another thread.

You can also split the workers out from the front end server by running this:

- In one terminal:

```bash
$ daphne myproject.asgi:channel_layer --port 8000
```

- In another terminal:

```bash
$ python manage.py runworker
```

And now they are separate processes!