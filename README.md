# DrupalCampNYC Website

Anyone can install this website locally even without being involved in DrupalCampNYC, but the website will have no content and the frontpage will point to a nonexistent page node until you create content and change the frontpage configuration.

# DrupalCampNYC DevOps

[drupalcamp.nyc](https://www.drupalcamp.nyc) is hosted on Pantheon. You need to be a member of the [DrupalCampNYC Pantheon site's "Team"](https://dashboard.pantheon.io/sites/36d6210e-0ea0-4579-9a00-a8d3ef891b81) (link only works if you are a member) to be able to:
* Make a manual backup of the site
* Download a copy of the site's database and/or files (sites/default/files)
* Deploy to Pantheon's Test or Stage environment

[Our canonical git repository](https://github.com/Drupal-NYC/drupalcampnyc) is on GitHub. You need to be a member of the GitHub Drupal-NYC organization's [DrupalCamp team](https://github.com/orgs/Drupal-NYC/teams/drupalcamp) (link only works if you are a member) in order to:
* Push changes to the master branch
* Approve pull requests

# Tome Netlify Demo (for DevOps Summit)

## Local Drupal + Netlify

This guide will describe how you can use traditional Drupal hosting, Tome Static, and Netlify to have a normal content editing experience while still running static html on your public-facing site.

First, you'll need to create a local development environment. For this demo, I'll use Lando since it's the local dev environment for the DrupalCampNYC site.

(Run `lando start` and `lando pull` on WSL2.)

Assuming you have your local Drupal 8 up and running, run this command from where your site's "composer.json" file lives: "composer require drupal/tome drupal/tome_netlify".

Edit your site "settings.php" file and set the "tome_static_directory" setting to a writeable directory on your hosting provider.

Tome documentation recommends adding the following block of code for sites hosted on Pantheon:

```php
if (isset($_ENV['PANTHEON_ENVIRONMENT'])) {
  $settings['tome_static_directory'] = 'sites/default/files/private/tome_static';
}
```

However, we want the option to export directly from Lando, so we won't even check the Pantheon environment variable:

```php
// Configure Tome Static directory for Tome Netlify.
$settings['tome_static_directory'] = 'sites/default/files/private/tome_static';

```

The rest of the instructions are quoted verbatim from [Tome documentation](https://tome.fyi/docs/hosting/hosted-drupal-netlify/):

> Commit the changes and push to your remote repository if you're using version control.
>
> Create a Netlify account at https://app.netlify.com/signup
>
> Login to  your Drupal site as an administrator. Ensure you are using the HTTPS URL of your Drupal site, as Netlify pushes will fail when using HTTP.
>
> Enable Tome Static and Tome Netlify at /admin/modules. You're enabling Tome Static and not Tome since you don't need any of the "store your content in Git" features that Tome Sync provides.
>
> Visit `/admin/config/tome/static/generate` and generate your first static site.
>
> Visit `/admin/config/tome/static/download` and download the static build. Extract the generated .tar.gz file.
>
> Visit https://app.netlify.com/account/sites and drag+drop your static build folder to the dropzone at the bottom of the screen. This will create a new Netlify site that isn't tracked in Git.
>
> Click into your new site in the Netlify UI, then click "Settings". Copy your site-specific "API ID" from there.
>
> Visit https://app.netlify.com/account/applications and generate a new personal access token. Copy your token as it can not be retrieved after viewing it the first time.
>
> Visit `/admin/config/services/tome_netlify/settings` and enter your token and site ID, then save the form.
>
> Visit `/admin/config/tome/netlify/send` and click Deploy.
>
> Wait a few minutes after a deploy is complete for Netlify to pick up the new build, then preview it using the link provided. Builds sent to Netlify only include files that have changed since the last build, which should make them super-duper fast.
>
> When satisfied with the deploy, publish the draft on Netlify at "Deploys" when viewing your site in the UI.
>
> You're done!
>
> While this guide may seem daunting, I think this hosting setup is the most reasonable for Drupal users who don't want to change the way content editors use Drupal, but still want to run a static site on the frontend.

# Credits

We are grateful to Drupal Europe 2018 for providing their [codebase](https://www.drupal.org/project/drupaleurope_website), which we adopted and improved upon.

# Collaborate
To collaborate in real-time, please join us in the #camp-website channel of [DrupalNYC Slack](https://www.drupalnyc.org/slack).

To collaborate with us and other camps on a shared codebase for future camps, please join us in the #event-website-starterkit channel of the [Drupal Event Organizers](https://www.drupal.org/community/event-organizers) Slack.

We encourage merge requests.
