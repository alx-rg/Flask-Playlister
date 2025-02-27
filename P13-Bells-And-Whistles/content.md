# Bells and Whistles

If you're looking for some extra challenges, there are a few more things we can do to tighten up our website and make it look more like the big leagues. _For the first two challenges, you will have to write some or all of the code on your own._ Let's get started!

# Displaying "Created At" Time

Let's display a "timestamp" for when a Playlist was created that looks like this: `Created on August 18, 2019 at 2:03 PM`.

First we will need to add a `created_at` field to our data model and store it in MongoDB when a playlist is created. To do this, we can use the `datetime` library. Read the documentation for [the `now()` method](https://docs.python.org/3.8/library/datetime.html#datetime.datetime.now), as it should be able to help us here...

Let's do that now.

Update the 'show' route in `app.py` to add a timestamp indicating when the playlist was created:

```python
from datetime import datetime
...
@app.route('/playlists', methods=['POST'])
def playlists_submit():
    """Submit a new playlist."""
    video_ids = request.form.get('video_ids').split()
    videos = video_url_creator(video_ids)
    playlist = {
        # TODO: edit the fields here to include created_at
    }
    print(playlist)
    playlist_id = playlists.insert_one(playlist).inserted_id
    return redirect(url_for('playlists_show', playlist_id=playlist_id))
```

Now we can display that `created_at` timestamp in our html. But remember, not all of our playlists will have a `created_at` field. _How can we make sure that we only display the `created_at` field if it exists in the playlist?_:

Update `templates/playlists_show.html` to include the `created_at` field right below the `title`. Make sure to use `<p class='text-muted'>` for the field:

Now create a new playlist and see what is displayed.

If the field is displaying, it shouldn't look exactly like what we want. What you see there is called a **Unix timestamp.**

`2019-08-19 05:14:58.038000`

It _technically_ says the date and time when the playlist was created, but it isn't very readable for humans!

# Formatting Timestamps

To parse our `datetime` object into something more readable, let's use its `strftime` function. We can specify what format we want our `datetime` to be in using **format codes** such as `%Y`, `%m`, and `%d` to represent the year, month, and day respectively.

Before continuing, make sure to read the [`strftime` documentation](https://docs.python.org/3.8/library/datetime.html#strftime-strptime-behavior), _especially around format codes_, as these will be key in order to make things human readable.

Let's try it out now!

Update the `playlists_show` template to display a formatted `playlist.created_at` string:

Reload your browser and check the timestamp. Pretty cool, huh?

# Adding a Footer

Now let's add a footer (brought to you by [mdbootstrap.com](mdbootstrap.com)). Add the following code after the `{% block content %}` tag but before the `</body>` tag or any `<script>` tags.

Add the following code after the `{% block content %}` tag, but before the `</body>` tag or any `<script>` tags in `templates/base.html`.

```html
<!-- templates/base.html -->
...

<!-- Footer -->
<footer class='page-footer font-small blue pt-4'>
    <div class='container-fluid text-center'>
         <div class='row'>
         <hr class='clearfix w-100 d-md-none pb-3'>
         <div class='col-md-6 mb-md-0 mb-3'>
             <h5 class='text-uppercase'>Links</h5>
             <ul class='list-unstyled'>
                 <li><a href='#!'>Link 1</a></li>
                 <li><a href='#!'>Link 2</a></li>
                 <li><a href='#!'>Link 3</a></li>
                 <li><a href='#!'>Link 4</a></li>
             </ul>
         </div>
         <div class='col-md-6 mb-md-0 mb-3'>
             <h5 class='text-uppercase'>Links</h5>
             <ul class='list-unstyled'>
                 <li><a href='#!'>Link 1</a></li>
                 <li><a href='#!'>Link 2</a></li>
                 <li><a href='#!'>Link 3</a></li>
                 <li><a href='#!'>Link 4</a></li>
             </ul>
         </div>
         </div>
    </div>
    <div class='footer-copyright text-center py-3'>© 2018 Copyright:
       <a href='https://mdbootstrap.com/education/bootstrap/'> MDBootstrap.com</a>
    </div>
</footer>

...
```

Beautiful. Make any visual changes you like!

# Adding a Bootstrap Theme

Now if we want to customize our style a little bit, we can add a free bootstrap theme. One popular website for finding these is called [Bootswatch](https://bootswatch.com/).

You can pick whichever you like, but for these instructions we'll use the "flatly" theme.

Go to [Bootswatch](https://bootswatch.com/) and select a theme from the `Themes` dropdown

Select the dropdown of the name of the style you selected. For example, if you like the `Flatly` style, you should see a `Flatly` dropdown as the last nav bar item on the left

Select the `bootstrap.min.css` option in the dropdown
Copy the URL into your `<head>` tag to include the style you want. Make sure that this theme comes AFTER your link to bootstrap itself. Otherwise it won't work.

```html
<head>
  ...

    <link rel='stylesheet' href='https://bootswatch.com/4/flatly/bootstrap.min.css'>
</head>
```

Now you can mess around with the CSS classes you have previously applied to achieve the look that you want!. Note that this will cause some areas of the app to look a _lot_ different from what we initially did. But all of it is adjustable with some additional CSS!

# Now Commit

```bash
$ git add .
$ git commit -m 'added created_at, footer, and bootstrap theme'
$ git push
```

Congrats! You've completed the tutorial :)
