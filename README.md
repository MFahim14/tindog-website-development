# tindog-website-development
<h2>Introduction:</h2>

Developed a TinDog website using Flask, incorporating dynamic content rendering, responsive design, and API integration. Key features include a structured homepage, dynamic header/footer management, functional About and Contact pages, and blog post display using data fetched from an API.

<h2>Explanation:</h2>

This Flask web application serves as a dynamic blog website that retrieves and displays blog posts from an external API. Here's a step-by-step explanation:

<b>1. Importing Required Libraries:</b>
```
from flask import Flask, render_template
import requests
```
```Flask:``` is imported to create the web application.

```render_template:``` is used to render HTML templates.

```requests:``` is used to fetch data from the external API.

<b>2. Fetching Blog Data:</b>
```
posts = requests.get("https://api.npoint.io/c790b4d5cab58020d391").json()
```

A GET request is made to an external API to fetch the blog posts data.

'''.json()''' converts the response into a Python list of dictionaries, where each dictionary represents a blog post with attributes such as title, content, and ID.

<b>3. Setting up the Flask App:</b>
'''
app = Flask(__name__)
'''

Initializes the Flask application. The ```__name__``` argument refers to the name of the current Python module, allowing Flask to understand the app's scope.

<b>4. Defining Routes:

Route 1: ```'/'``` (Homepage):</b>
```
@app.route('/')
def get_all_posts():
    return render_template("index.html", all_posts=posts)
```
This route serves the homepage.
It passes the posts data (list of blog posts) to the ```index.html``` template using ```all_posts```.

<b>Route 2: ```/about```:</b>
```
@app.route("/about")
def about():
    return render_template("about.html")
```
This route renders the About page using ```about.html```.

<b>Route 3: ```/contact```:</b>
```
@app.route("/contact")
def contact():
    return render_template("contact.html")
```
This route renders the Contact page using ```contact.html```.

<b>Route 4: ```/post/<int:index>```:</b>
```
@app.route("/post/<int:index>")
def show_post(index):
    requested_post = None
    for blog_post in posts:
        if blog_post["id"] == index:
            requested_post = blog_post
    return render_template("post.html", post=requested_post)
```
This route dynamically serves individual blog posts based on the ```index``` parameter in the URL.

It loops through the posts list to find the blog post with the matching ```id```.

The ```post.html``` template is rendered with the matching blog post passed as post.

<b>5. Running the Application:</b>
```
if __name__ == "__main__":
    app.run(debug=True, port=5001)
```
If the script is run directly, the Flask app will start with ```debug=True``` (helpful for debugging).
The app will run on port ```5001```.

<h2>Directions:</h2>

<b>1. Homepage:</b> When you visit ```'/'```, all blog posts from the API will be displayed.

<b>2. About Page:</b> Visit ```/about``` to access the About page.

<b>3. Contact Page:</b> Visit ```/contact``` to access the Contact page.

<b>4. Individual Blog Post:</b> Visiting ```/post/<index>``` (e.g., ```/post/1```) will display the blog post with the corresponding ID.

<h2>Explanation Of Routes(Directions):</h2>

<h2>1.Homepage: </h2>

This is an HTML template, written with Jinja syntax, which is used in Flask for rendering dynamic content. The template is structured to display a blog page with posts, using Bootstrap for styling and layout. Here's a breakdown of the code:

<b>1. Including the Header:</b>

```
{% include "header.html" %}
```

This line includes the ```header.html``` template at the top of the page, which typically contains the ```<head>``` section of the HTML, any CSS or JS links, and possibly a navigation bar.

<b>2. Page Header:</b>
```
<header class="masthead" style="background-image: url('https://images.unsplash.com/photo-1470092306007-055b6797ca72?ixlib=rb-1.2.1&auto=format&fit=crop&w=668&q=80')">
    <div class="container position-relative px-4 px-lg-5">
        <div class="row gx-4 gx-lg-5 justify-content-center">
            <div class="col-md-10 col-lg-8 col-xl-7">
                <div class="site-heading">
                    <h1>Clean Blog</h1>
                    <span class="subheading">A Blog Theme by Start Bootstrap</span>
                </div>
            </div>
        </div>
    </div>
</header>
```
This section defines the page header or masthead, which is a full-width background image taken from an external source (Unsplash).

The ```style="background-image: url(...)"``` sets the background image for the header.

Inside the header, there is a container and grid layout (using Bootstrap's ```container```, ```row```, and ```col``` classes).

The text in the header includes the blog title ("Clean Blog") and a subtitle ("A Blog Theme by Start Bootstrap").

<b>3. Main Content Area:</b>
```
<div class="container px-4 px-lg-5">
    <div class="row gx-4 gx-lg-5 justify-content-center">
        <div class="col-md-10 col-lg-8 col-xl-7">
```
This section is the main content area of the blog, wrapped in a Bootstrap container to ensure responsive behavior. The grid layout is used to center the content ('''justify-content-center''').

<b>4. Blog Posts Loop:</b>
```
{% for post in all_posts %}
    <div class="post-preview">
        <a href="{{ url_for('show_post', index=post.id) }}">
            <h2 class="post-title">{{ post.title }}</h2>
            <h3 class="post-subtitle">{{ post.subtitle }}</h3>
        </a>
        <p class="post-meta">
            Posted by
            <a href="#!">{{ post.author }}</a>
            on {{ post.date }}
        </p>
    </div>
    <hr class="my-4" />
{% endfor %}
```
This loop uses Jinja syntax to iterate over the ```all_posts``` list (passed from the Flask app).

Each iteration renders a blog post preview.

The ```url_for('show_post', index=post.id)``` generates a dynamic URL that links to the full blog post page (```/post/<id>```).

Inside each post preview:

Title: Displayed inside an ```<h2>``` element.

Subtitle: Displayed inside an ```<h3>``` element.

Post meta: Information about the author and date of the post is displayed below the title and subtitle.

After each post, a horizontal divider (```<hr>```) separates the posts.


<b>5. Older Posts Button:</b>
```
<div class="d-flex justify-content-end mb-4">
    <a class="btn btn-primary text-uppercase" href="#!">Older Posts →</a>
</div>
```
This is a "Older Posts" button aligned to the right (```justify-content-end```).
The button is styled using Bootstrap classes ```btn btn-primary``` and displays "Older Posts →". It currently links to ```#```, which is a placeholder.

<b>6. Including the Footer:</b>
```
{% include "footer.html" %}
```
This line includes the ```footer.html``` template, which typically contains the footer content and any closing tags like ```</body>``` and ```</html>```.

<h2>2. About Page:</h2>

This is an HTML template for an "About Me" page, structured using Jinja syntax, and styled using Bootstrap for a clean, responsive layout. Here's a detailed explanation of the code:

<b>1. Including the Header:</b>
```
{% include "header.html" %}
```
This line includes the ```header.html``` file at the top of the page, which typically contains the standard ```<head>``` section, CSS files, and potentially a navigation bar.

<b>2. Page Header Section:</b>
```
<header
  class="masthead"
  style="background-image: url('../static/assets/img/about-bg.jpg')"
>
```

This creates a full-width header section with a background image.

The ```background-image``` is set to ```about-bg.jpg```, located in the ```static/assets/img``` folder.

Bootstrap classes like ```masthead``` and ```container``` are used for styling and layout, ensuring that the content within the header is centered and properly spaced.

<b>3. Header Content:</b>
```
<div class="container position-relative px-4 px-lg-5">
  <div class="row gx-4 gx-lg-5 justify-content-center">
    <div class="col-md-10 col-lg-8 col-xl-7">
      <div class="page-heading">
        <h1>About Me</h1>
        <span class="subheading">This is what I do.</span>
      </div>
    </div>
  </div>
</div>
```
Inside the header, the content is laid out using Bootstrap's grid system (```container```, ```row```, and ```col``` classes).

The header displays a title ("About Me") and a subtitle ("This is what I do") inside a ```page-heading``` div.

The content is centered using ```justify-content-center``` and spans multiple column sizes for responsiveness (```col-md-10 col-lg-8 col-xl-7```).

<b>4. Main Content Section:</b>
```
<main class="mb-4">
  <div class="container px-4 px-lg-5">
    <div class="row gx-4 gx-lg-5 justify-content-center">
      <div class="col-md-10 col-lg-8 col-xl-7">
```
This section is wrapped in a ```main``` element with a margin at the bottom (```mb-4```).

The content is centered using Bootstrap's grid system again, and it spans a column of ```col-md-10 col-lg-8 col-xl-7``` for responsiveness.

<b>5. Text Content:</b>
```
<p>
  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Saepe
  nostrum ullam eveniet pariatur voluptates odit, fuga atque ea nobis
  sit soluta odio, adipisci quas excepturi maxime quae totam ducimus
  consectetur?
</p>
<p>
  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Eius
  praesentium recusandae illo eaque architecto error, repellendus iusto
  reprehenderit, doloribus, minus sunt. Numquam at quae voluptatum in
  officia voluptas voluptatibus, minus!
</p>
<p>
  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aut
  consequuntur magnam, excepturi aliquid ex itaque esse est vero natus
  quae optio aperiam soluta voluptatibus corporis atque iste neque sit
  tempora!
</p>
```

This section contains three paragraphs of placeholder text (Lorem ipsum) describing what the page is about. These paragraphs simulate a description of the person's work or background.

<b>6. Including the Footer:</b>
```
{% include "footer.html" %}
```
This line includes the ```footer.html``` file at the bottom of the page, which typically contains the footer section, closing HTML tags, and possibly additional JavaScript imports.

<h2>3. Contact Page:</h2>

This is an HTML template for a Contact Me page. It uses Bootstrap for layout and styling, with a contact form to allow users to send messages. Below is a detailed explanation of the code:

<b>1. Including the Header:</b>
```
{% include "header.html" %}
```
This line includes the ```header.html``` file, which typically contains the ```<head>``` section with CSS, JS files, and possibly a navigation bar.

<b>2. Page Header Section:</b>
```
<header class="masthead" style="background-image: url('../static/assets/img/contact-bg.jpg')">
```

This creates a full-width header for the page with a background image (```contact-bg.jpg```), located in the ```static/assets/img``` folder.

The ```masthead``` class (from Bootstrap) ensures the header spans the full width and height, making it visually prominent.

<b>Header Content:</b>
```
<div class="container position-relative px-4 px-lg-5">
    <div class="row gx-4 gx-lg-5 justify-content-center">
        <div class="col-md-10 col-lg-8 col-xl-7">
            <div class="page-heading">
                <h1>Contact Me</h1>
                <span class="subheading">Have questions? I have answers.</span>
            </div>
        </div>
    </div>
</div>
```

Bootstrap's grid system is used here for responsive layout, ensuring that the content (heading and subheading) is centered.

The header displays:

Title: "Contact Me".

Subtitle: "Have questions? I have answers."

The content is centered using ```justify-content-center``` and spans multiple column sizes (```col-md-10 col-lg-8 col-xl-7```), ensuring responsiveness across different devices.

<b>3. Main Content Section:</b>
```
<main class="mb-4">
    <div class="container px-4 px-lg-5">
        <div class="row gx-4 gx-lg-5 justify-content-center">
            <div class="col-md-10 col-lg-8 col-xl-7">
```

The main section contains the actual content, such as instructions and a contact form.

The content is placed in a Bootstrap grid to ensure responsiveness. The ```col-md-10``` class defines the width for medium-sized screens and adjusts accordingly for larger devices.

<b>4. Description:</b>
```
<p>
    Want to get in touch? Fill out the form below to send me a message and I will get back to you as soon as possible!
</p>
```

This paragraph provides a short description, inviting users to fill out the contact form.

<b>5. Contact Form:</b>
```
<form id="contactForm" data-sb-form-api-token="API_TOKEN">
```
This starts a form element with the ID ```contactForm```. The ```data-sb-form-api-token``` attribute is used for SB Forms integration (a form processing service provided by Start Bootstrap).
Note: This form is not functional without signing up for SB Forms and obtaining an API token.

<b>Name Field:</b>
```
<div class="form-floating">
    <input class="form-control" id="name" type="text" placeholder="Enter your name..." data-sb-validations="required" />
    <label for="name">Name</label>
    <div class="invalid-feedback" data-sb-feedback="name:required">A name is required.</div>
</div>
```
The ```form-floating``` class is used for floating labels, which ensures the label appears inside the input field and "floats" above it once the user starts typing.

The name field is required (```data-sb-validations="required"```), and there is an error message (```invalid-feedback```) that displays if the user submits without entering a name.

<b>Email Field:</b>
```
<div class="form-floating">
    <input class="form-control" id="email" type="email" placeholder="Enter your email..." data-sb-validations="required,email" />
    <label for="email">Email address</label>
    <div class="invalid-feedback" data-sb-feedback="email:required">An email is required.</div>
    <div class="invalid-feedback" data-sb-feedback="email:email">Email is not valid.</div>
</div>
```

The email field is validated for correct email format. It has two error messages:

If the field is left empty (```email:required```).

If the entered email is not valid (```email:email```).

<b>Phone Field:</b>
```
<div class="form-floating">
    <input class="form-control" id="phone" type="tel" placeholder="Enter your phone number..." data-sb-validations="required" />
    <label for="phone">Phone Number</label>
    <div class="invalid-feedback" data-sb-feedback="phone:required">A phone number is required.</div>
</div>
```
The phone number field is required, with appropriate validation and feedback.

<b>Message Field:</b>
```
<div class="form-floating">
    <textarea class="form-control" id="message" placeholder="Enter your message here..." style="height: 12rem" data-sb-validations="required"></textarea>
    <label for="message">Message</label>
    <div class="invalid-feedback" data-sb-feedback="message:required">A message is required.</div>
</div>
```
The message field is a ```textarea``` element with a height of 12 rem. It is also required, and an error message is shown if left empty.

<b>6. Form Submission Messages:</b>
```
<!-- Submit success message -->
<div class="d-none" id="submitSuccessMessage">
    <div class="text-center mb-3">
        <div class="fw-bolder">Form submission successful!</div>
        To activate this form, sign up at
        <a href="https://startbootstrap.com/solution/contact-forms">https://startbootstrap.com/solution/contact-forms</a>
    </div>
</div>
```
This section defines a success message that will be shown when the form is successfully submitted. By default, it is hidden (```d-none```).
```
<!-- Submit error message -->
<div class="d-none" id="submitErrorMessage">
    <div class="text-center text-danger mb-3">Error sending message!</div>
</div>
```
This section defines an error message that will be shown if there is an issue submitting the form.

<b>7. Submit Button:</b>
```
<button class="btn btn-primary text-uppercase disabled" id="submitButton" type="submit">Send</button>
```
This is the submit button, styled with Bootstrap's ```btn btn-primary``` class.

The button starts as disabled (```disabled```) because form validation must first be met for the button to become active.

<b>8. Including the Footer:</b>
```
{% include "footer.html" %}
```
The ```footer.html``` template is included at the bottom of the page, typically containing the page footer and any closing tags.

<h2>4. Individual Blog Post:</h2>

This HTML template renders a blog post page using the Flask templating engine, Jinja2. The content of the blog post (such as the title, subtitle, author, and body) is dynamically inserted into the template using variables (```{{ }}```) provided by the Flask backend.

Here’s a breakdown of the code:

<b>1. Including the Header:</b>
```
{% include "header.html" %}
```

This line includes the contents of the ```header.html``` file, which typically contains the ```<head>``` section, navigation bar, and any linked stylesheets or JavaScript files.

<b>2. Page Header Section:</b>
```
<header class="masthead" style="background-image: url('{{ post.image_url }}')">
```

This is the main header for the blog post page. The background image is dynamically set using ```post.image_url```, which is passed from the Flask application.

The ```masthead``` class is likely from Bootstrap and ensures that the header section is styled prominently, taking up significant space and displaying the background image.

<b>Header Content:</b>
```
<div class="container position-relative px-4 px-lg-5">
    <div class="row gx-4 gx-lg-5 justify-content-center">
        <div class="col-md-10 col-lg-8 col-xl-7">
            <div class="post-heading">
                <h1>{{ post.title }}</h1>
                <h2 class="subheading">{{ post.subtitle }}</h2>
                <span class="meta">Posted by
                    <a href="#">{{ post.author }}</a>
                    on {{ post.date }}
                </span>
            </div>
        </div>
    </div>
</div>
```
This content is displayed within the header section:

Title (```{{ post.title }}```): The title of the blog post is displayed dynamically.

Subtitle (```{{ post.subtitle }}```): A subtitle is rendered just below the title.

Meta information: It shows the author’s name (```{{ post.author }}```) and the date of publication (```{{ post.date }}```).

Bootstrap classes such as ```container```, ```row```, ```col-md-10```, etc., are used for a responsive grid layout.

<b>3. Post Content:</b>
```
<article class="mb-4">
    <div class="container px-4 px-lg-5">
        <div class="row gx-4 gx-lg-5 justify-content-center">
            <div class="col-md-10 col-lg-8 col-xl-7">
                <p>{{ post.body }}
            </div>
        </div>
    </div>
</article>
```

This section is where the main content of the blog post is displayed.

The ```{{ post.body }}``` contains the actual text of the blog post, dynamically rendered.

This section is wrapped in Bootstrap’s responsive grid system (```container```, ```row```, ```col-md-10```, etc.) to ensure proper formatting across different screen sizes.

<b>4. Including the Footer:</b>
```
{% include "footer.html" %}
```
This line includes the ```footer.html``` file, which contains the footer of the webpage. It likely contains copyright information, links, or additional content for the bottom of the page.
