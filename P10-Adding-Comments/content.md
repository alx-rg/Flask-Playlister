# Adding Comments

At this point you should have a functioning site that creates and saves Playlists to a database! This system manages a *single resource*, which we've called a `Playlist`. In this section you will add a new resource which will represent comments that visitors can add to Playlists.

Comments will be treated like Playlists: they will have a model that defines them, and they will be stored as documents in the database.

# Create a partial for the comment form

Follow the same approach used in the tutorial, start with what users will see. Users reading a Playlist may want to write a comment and read comments posted by other users. Adding comments will require a form to enter the comment.

Putting the comment form in a partial will make your code modular and help keep it organized.

Make a new file `templates/partials/comment_form.html`:

```html
<!-- templates/partials/comment_form.html -->

<form>
    <fieldset>
        <legend>Add a Comment</legend>
        <!-- TITLE -->
        <div class='form-group'>
            <label for='comment-title'>Title</label><br>
            <input class='form-control'
                id='comment-title'
                type='text'
                name='title'
                placeholder='Title'/>
        </div>

        <!-- DESCRIPTION -->
        <div class='form-group'>
            <label for='comment-content'>Description</label><br>
            <textarea class='form-control'
                id='comment-content'
                name='content'
                rows='10'
                placeholder='Your comment...'></textarea>
        </div>
    </fieldset>

    <!-- BUTTON -->
    <div class='form-group'>
        <button class='btn btn-primary' type='submit'>Save Comment</button>
    </div>
</form>
```

Notice you used the same class names and markup used in the Playlists Form. This form only has three form elements:

1. One input for the title of the comment
1. A text area for the content of the comment
1. A button to submit the form.

# Display the Comment

Add the partial to the bottom of `templates/playlists_show.html`.

```html
<!-- templates/playlists_show.html -->

<a href='/'>Back to Home</a>
<div class='row'>
    <div class='col-sm-6 col-sm-offset-3'>
        <h1>{{ playlist.title }}</h1>
        <h2>{{ playlist.description }}</h2>
        {% for video in playlist.videos %}
            <div class="card"><div class="card-body">
                <iframe width="420" height="315" src="{{ video }}"></iframe>
            </div></div>
        {% endfor %}
        <p><a href='/playlists/{{ playlist._id }}/edit'>Edit</a></p>
        <p><form method='POST' action='/playlists/{{playlist._id}}'>
            <button class='btn btn-primary' type='submit'>Delete</button>
        </form></p>

        <hr>

    <!-- Add your new partial here: -->
    <!-- Comment form -->
    {% include 'partials/comment_form.html' %}

    </div>
</div>
```

For now the form action is empty. You should be able to see the form but, nothing will happen if you click submit. We need to add a route for that! We'll do that next, but let's commit what we have so far.

# Now Commit

```bash
$ git add .
$ git commit -m 'Comment form added'
$ git push
```

# Next

Click [here](../P11-Adding-Route-For-Comments/content.md) to move onto the next section about adding a route for the comments.
