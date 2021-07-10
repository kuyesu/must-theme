# must-theme

# Example 1: Changing Colors and Fonts


 A sample theme named “red-theme” comes pre-installed on your Open edX installation. You’ll find the theme files here: /edx/app/edxapp/edx-platform/themes/. Of the thousands of classes, attributes and variable assignments that makeup the base theme, the Red Them makes only two modifications, but the results are profound (and really, really ugly). First, the theme’s primary color is changed from blue to red, and second, the font family is changed from San Serif to Comic Sans, which is arguably one of the most heinous fonts ever designed. 🤢 But I digress.
The Red Theme demonstrates the ease and simplicity of overriding the Open edX UI’s default theme settings with custom values. The only problem with the Red Them is that there is absolutely no explanation whatsoever of how it works.
To avoid modifying the original system files, I suggest that you create a new theme repository from a copy of the Red Theme. Here are the key points and principals at work in the Red Theme (and the Dark Theme as well):
The most important elements of the theme come from the file /edx/app/edxapp/edx-platform/themes/red-theme/lms/static/sass/partials/lms/theme/_variables.scss. This file actually originates from Bootstrap and is used exclusively for overriding Bootstrap theme variables. I highly encourage you to Get Acquainted with Bootstrap Theming as this will save you countless hours of pain and struggle.
The Red Theme file _variable.scss is intended to be used to override any of the dozens of variables assigned in /edx/app/edxapp/edx-platform/lms/static/sass/partials/lms/theme/_variables-v1.scss.
The edX designers packaged their base Open edX Bootstrap theme into a Node NPM package which contains the more important variable declarations and assignments. The node package is located here: /edx/app/edxapp/edx-platform/node_modules/@edx/edx-bootstrap/sass/open-edx, and its only two files are _variables.scss and _theme.scss. If you’re trying to change theme colors then this file contains all of the salient variable names and their default values. Here is a snapshot of the contents of this file beginning around row 65:


<img width="640" alt="bootstrap-edx-theme-variables" src="https://user-images.githubusercontent.com/69388140/125160461-f76edf00-e14a-11eb-94d2-b578de2f7849.png">

# Example 2: Add a Custom Image or Graphic to a Page
Add the custom image to your theme repository, in the lms/static/images/ folder
<img width="578" alt="my-awesome-image" src="https://user-images.githubusercontent.com/69388140/125160500-3ac94d80-e14b-11eb-9d7f-4607038475f1.png">
Here is an example of a Mako template reference to an image in your custom theme
img class="logo" src="${static.url("images/my-awesome-image.png")}" alt="My Awesome Image"/>
Recompile static assets:
sudo wget https://github.com/Kuyesu/edx.scripts/blob/master/edx.compile-assets.sh | bash


# Example 3: Modify CSS styling of existing LMS features
Coming soon! 😬

# Example 4: Enable Bootstrap on the Home Page
Bootstrap is included in the native build of Open edX Ginkgo and later. However, it is selectively enabled based on a flag variable contained in each page’s Django view. Bootstrap is not enabled on the homepage of the LMS, however, you can manually add the necessary CSS and javascript libraries to the index.html Mako template. You can see a functional example here: https://github.com/Kuyesu/edx.custom-theme/…/index.html.

Lets take a closer look at the necessary modifications to this Mako template.

On row 10 we test the Django variable “uses_bootstrap” to see if Bootstrap is included in the WebPack version of minified CSS linked to the page via the LMS’ production system. If it exists then we do nothing. Otherwise, we add a link to a CDN-supplied Bootstrap library. I copied this link from the Bootstrap home page “Get Started” link:

1
2
3
4
<head>
<!-- McDaniel Oct-2018: add the Bootstrap library, unless it's already included from webpack -->
% if not uses_bootstrap:link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">% endif
</head>
On row 86 we use a similar coding pattern to optionally add the Bootstrap javascript library if its not already present on the page. I actually copied this code fragment directly from the bottom of /edx/app/edxapp/edx-platform/lms/templates/main.html.

1
2
3
4
5
% if not uses_bootstrap:
<!-- McDaniel Oct-2018: add the Bootstrap javascript library, unless it's already included from webpack -->
## xss-lint: disable=mako-invalid-js-filter
script type="text/javascript" src="${static.url('common/js/vendor/bootstrap.bundle.js')}">
% endif
With the CSS and Javascript libraries loaded on our homepage we can now add Bootstrap components to our page. As an added bonus, the Bootstrap components that we add to our homepage will reflect any styling modifications that we might have added to theme/themes/custom-theme/lms/static/sass/partials/lms/theme/_variables.scss. In the example home page below we can see two sample Bootstrap alert boxes, one of which is dismissible (confirming that the Javascript library is working), and both of which are inheriting a few custom theme colors.


# III. Annex: Official Open edX Styling Documentation
I found the following styling documentation in /edx/app/edxapp/edx-platform/doc/. This brief narrative sheds some light on the direction of CSS styling in future releases of the platform.
