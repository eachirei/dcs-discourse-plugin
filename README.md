# dcs-discourse-plugin

A Discourse plugin that allows to integrate your website or web app with
Discourse. Part of the [Docuss](https://github.com/sylque/docuss) solution.

**This project is not active anymore.** Fixes are provided to existing users, but I stopped working on new features. You might want to check  [DiscPage](https://github.com/sylque/discpage), which is somehow a simplified version of Docuss.

## Demos

See [here](https://github.com/sylque/docuss).

## Discourse Setup

### 1. Install the plugin in Discourse

The plugin’s repository url is:

```
https://github.com/sylque/dcs-discourse-plugin.git
```

Step by step guide, based on Discourse
[official documentation](https://meta.discourse.org/t/install-plugins-in-discourse):

1. Access your Discourse server
2. Type `cd /var/discourse`
3. Type `sudo nano containers/app.yml`
4. Arrow down until you see this:

```
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
           - git clone https://github.com/discourse/docker_manager.git
```

5. Paste `- git clone https://github.com/sylque/dcs-discourse-plugin.git`
   beneath that, so that all together it looks like this:

```
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
           - git clone https://github.com/discourse/docker_manager.git
           - git clone https://github.com/sylque/dcs-discourse-plugin.git
```

6. Hit `control + O` then Enter (to save `app.yml`)

7. Hit `control + X` (to exit `app.yml`)

8. Type `sudo ./launcher rebuild app` to rebuild Discourse

### 2. Set Discourse settings

In the Discourse Admin panel, open the Settings page and set those settings:

- `tagging enabled` &rightarrow; checked
- `min trust to create tag` &rightarrow; `0: new user`
- `allow duplicate topic titles` &rightarrow; checked
- `docuss enabled` &rightarrow; checked
- `docuss website json file` &rightarrow; one or several urls, each one pointing
  to a json file describing a website or web app. The structure of the file is
  described [here](https://github.com/sylque/dcs-website-schema). If you are new
  to Docuss and just want to see if the plugin works correctly, enter this url:
  `https://sylque.github.io/dcs-client/demos/mustacchio/dcs-website.json`.
  **Important note**: if your Discourse instance is accessed through HTTPS, then
  your json files must be accessed through HTTPS also.

If your Discourse instance doesn't use tags (i.e. if `tagging enabled` was
unchecked before you checked it as part of this setup), set this additional
setting:

- `docuss hide tags` &rightarrow; checked

### 3. Create the required tags

Select a public topic (or create a new one), edit it, and add those two tags:

- `dcs-comment`
- `dcs-discuss`

Discourse will ask if you want to create them: answer yes.

~~Now you can safely delete the topic if you want, the tags will remain~~. Nope,
sometimes it doesn't work. You better keep the topic.

## Website Navigation

Now that your website is displayed within Discourse, you might want to
reconsider your navigation/menu system. You can keep it in your website, you can
move it to Discourse, or you can do a bit of both.

At the very minimum, in your website, you need to update the links to your
forum. Change them to `http://www.mydiscourse.org/latest` (instead of
`http://www.mydiscourse.org`).

Discourse custom navigation is out of the scope of this plugin. To learn more
about it, see
[Best way to customize the header](https://meta.discourse.org/t/best-way-to-customize-the-header/13368).
Just use the fact that, whether from a Discourse menu or from your website:

- `http://www.mydiscourse.org/` points to your home page,
- `http://www.mydiscourse.org/docuss/[pageName]` points to one of your website's
  page,
- `http://www.mydiscourse.org/latest` points to your forum.

## Additional Plugin Settings

- `docuss hide sugg topics`: hide suggested topics displayed at the bottom of
  Docuss topic pages.
- `docuss hide categories`: hide categories everywhere in Discourse. Notice that
  you can also set "header dropdown category count" to 0 to hide categories in
  the hamburger menu alone.
- `docuss hide tags`: hide tags everywhere in Discourse.
- `docuss hide hamburger menu`: hide the Discourse hamburger menu.

## Known limitations when using tags

Docuss relies on automatically generated tags to achieve its magic (generated
tags are of the form `dcs-xxxxxx-xxxxxx`). That is why, to be on the safe side,
it is advised _not_ to use tags in your Discourse community and set the above
`docuss hide tags` setting to true. To ease your decision, consider that Docuss
is a new way of categorizing topics ("per balloon", instead of "per category" or
"per tag"), so maybe tags are unnecessary with Docuss anyway.

However, if you decide to use tags in your Discourse community, Docuss will try
its best to hide its automatically generated tags. One area where Docuss doesn't
do a good job is the `tags` page, where Docuss tags will appear blank instead of
being hidden.

As an administrator, when troubleshooting tag issues, press `Alt+a` to disable
Docuss CSS and thus showing Docuss tags.

## FAQ

> When I create a topic for a Docuss page, the `category` field is hidden. Why
> is that?

User cannot select a category from a Docuss page. Categories are applied
automatically by Docuss, depending on webmaster predefined choices. The way
webmasters define categories depends on the Docuss Client library they use:

- `comToPlugin.js`: see
  [here](https://github.com/sylque/dcs-client/blob/master/comToPlugin.md#set-the-route-properties)
- `dcs-html-based.js`: see
  [here](https://github.com/sylque/dcs-client/blob/master/dcs-html-based.md)
- `dcs-decorator.js`: see
  [here](https://github.com/sylque/dcs-website-schema#file-format-reference)
  (search the page for "category")

## License

See [here](https://github.com/sylque/docuss#license).
