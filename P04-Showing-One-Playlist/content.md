# Show Route: See One Resource

We are building out all the **Resourceful Routes** for our `Playlist` resource.

| URL              | HTTP Verb | Action  |
|------------------|-----------|---------|
| /                | GET       | index   |
| /playlists/new     | GET       | new     |
| /playlists         | POST      | create  |
| /playlists/:id     | GET       | show    |

We've already completed the index, new, and create actions. Now you will add a show action that will display a single resource via its id.

Now let's setup the **show** action so we give each single playlist its own page and unique url path. This way we can _actually_ view the playlists we create!

# Show One Playlist

Remember always start with what the user will see and do. To create the show action, you will want to start by making a link to the playlist from our index action template. Your route has to follow the `/playlists/:id` structure.

MongoDB automatically creates an `_id` attribute on anything you save. So we can use that `_id` attribute for our `:id` in the route. This is called the **Url or Request Parameter** and we access it in Flask using a parameter inside of the route.

Update `templates/playlists_index.html` to the following:

```html
<!-- templates/playlists_index.html -->
{% extends 'base.html' %}

{% block content %}
<h1>Playlists</h1>

<a href='/playlists/new'>New Playlist</a>

{% for playlist in playlists %}
    <h2><a href='/playlists/{{ playlist._id }}'>{{ playlist.title }}</a></h2>
    <small>{{ playlist.description }}</small>
{% endfor %}
{% endblock %}
```

What happens if you click on one of those links? A friendly error! Let's do what it says and make the route.

Try this one yourself!

Add the `/playlists/<playlist_id>` route to `app.py`. For now, we just want it to return the string "My ID is P_ID", but replace P_ID with the actual `playlist_id`:

Now what happens if you go to that route? You should see the string stating the `playlist_id` in your logs!

# Get a Single Playlist from MongoDB

Ok, time to add a template with an actual `playlist` object! To get the playlist from our MongoDB database, we can use the `find_one()` method. We will have to convert the string value of its `_id` to an `ObjectId`.

Create the `/playlists/<playlist_id>` route in `app.py`:

```python
# Add this with the rest of your import statements
from bson.objectid import ObjectId

# ...

@app.route('/playlists/<playlist_id>')
def playlists_show(playlist_id):
    """Show a single playlist."""
    playlist = playlists.find_one({'_id': ObjectId(playlist_id)})
    return render_template('playlists_show.html', playlist=playlist)
```

Now if we go to the route, we'll see the error that no template `playlists_show` is found. Great! Let's make it.

Create `templates/playlists_show.html`:

```html
<!-- templates/playlists_show.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ playlist.title }}</h1>
<h2>{{ playlist.description }}</h2>
{% for video in playlist.videos %}
    <iframe width='420' height='315' src='{{ video }}'></iframe>
{% endfor %}
{% endblock %}
```

Now what do you see? All the links to playlists should work now!

# Add a Back Link

This is good, the show action is working, but there is a bit of a problem. Once you are on the show action page, you can't get back home.

Try this one on your own!

Let's fix that by putting in a "Back" link in `templates/playlists_show.html`. Above the `title`, place an `<a>` element that links back to the home page, and has the text "Back to Home"

That's better. What else could we do now that we have this show route?

# Update the Create Action's Redirect

It makes sense from the user's perspective that after we create a new playlist, we should be automatically redirected to it, no?

Try this one on your own too!

Change the create route (`'/playlists', methods=['POST']`) to redirect to the `playlists_show` path in `app.py`. If you get stuck, think back to how we built our `submit` route

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can see single playlists'
$ git push
```

Now our user experience is getting very smooth, and our code is getting more and more complete. Onward!

# Next

Click [here](../P05-Editing-A-Playlist/content.md) to move onto the next section about editing a playlist.
