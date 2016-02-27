# limnoria-gogs

limnoria-gogs is a plugin for [limnoria](https://github.com/ProgVal/Limnoria)
that provides support for [gogs](https://gogs.com) webhook notifications.
Currently it has the following features:

  - Support of push, tag, issue, comment and merge request events
  - Commands to manage subscribed projects per channel
  - Localization

### Installation

To install this plugin just copy its directory to the
`supybot.directories.plugins` directory of your limnoria instance and enable it
in your configuration file under `supybot.plugins`. For more information
checkout the [Supybot user
guide](http://doc.supybot.aperio.fr/en/latest/use/index.html).

### Configuration

The _limnoria-gogs_ plugin uses the build-in web service of Limnoria therefore
it listens on the address configured by `supybot.servers.http.hosts[4,6]` and
`supybot.servers.http.port`. For more information on the HTTP server of Limnoria
checkout the '[Using the HTTP
server](http://doc.supybot.aperio.fr/en/latest/use/httpserver.html)' chapter of
their documentation.

Depending on the configuration of your Limnoria instance and your web server the
plugin now listens on the following address where it accepts the network and the
channel as a parameter:

`http://<host>:<port>/gogs/<network>/<channel>`

The placeholders are defined as followed:

  - `<host>` - The host defined by the external IP of the service
  - `<port>` - The port that the HTTP server of Limnoria listens to
  - `<network>` - The network that the Limnoria instance is connected to
  - `<channel>` - The channel that the Limnoria instance is in

For instance if your bot is in the _OFTC_ network and in the _#limnoria-gogs_
channel, the plugin listens on the following URL for webhook notifications:

`http://limnoria.example.com:8080/gogs/OFTC/limnoria-gogs`

Now you need to add this address as a new webhook in the project settings of
your Gogs instance. Therefore you go to `Settings -> Webhooks`
and click `Add Web Hook` after you've entered the above address under URL and
selected the checkboxes for the types of notifications you want to be send to
the channel.

### Commands

- `gogs project add [<channel>] <project-slug> <project-host>` -
  This command subscribes a new project to the channel:
    - `[<channel>]` - The channel that should be used. _(Optional, defaults to the current channel)_
    - `<project-slug>` - The slug of the gogs project
    - `<project-host>` - The host of the gogs project

  Example: To subscribe the _example_project_ to the current channel you can run the following command: `gogs project add example_project https://gogs.example.com/foo/example_project`

- `gogs project remove [<channel>] <project-slug>` - This command removes a subscribed project from the channel:
    - `[<channel>]` - The channel that should be used. _(Optional, defaults to the current channel)_
    - `<project-slug` - The slug of the gogs project

- `gogs project list [<channel>]` - Lists the subscribed projects from the channel:
    - `[<channel>]` - The channel that should be used. _(Optional, defaults to the current channel)_

### Options

The following option can be set for each channel and defines the list of subscribed projects (this option should only be set by the commands of this plugin).

- `plugins.Gogs.projects` - Saves the subscribed project mappings _(Default: empty)_ **Readonly!**

In addition all the formats that are used to notify the channel about changes on the Gogs project can be configured:

- `plugins.Gogs.format.push` - The format that is used if a milestone has been created
- `plugins.Gogs.format.commit` - The format that is used if a milestone has been deleted
- `plugins.Gogs.format.tag` - The format that is used if a milestone has been changed
- `plugins.Gogs.format.issue-open` - The format that is used if an issue has been created
- `plugins.Gogs.format.issue-update` - The format that is used if an issue has been updated
- `plugins.Gogs.format.issue-close` - The format that is used if an issue has been closed
- `plugins.Gogs.format.issue-reopen` - The format that is used if an issue has been reopened
- `plugins.Gogs.format.merge-request-open` - The format that is used if an merge request has been created
- `plugins.Gogs.format.merge-request-update` - The format that is used if an merge request has been updated
- `plugins.Gogs.format.merge-request-close` - The format that is used if an merge request has been closed
- `plugins.Gogs.format.merge-request-reopen` - The format that is used if an merge request has been reopened
- `plugins.Gogs.format.merge-request-merge` - The format that is used if an merge request has been merged
- `plugins.Gogs.format.note-merge-request` - The format that is used if someone commented on a merge request
- `plugins.Gogs.format.note-commit` - The format that is used if someone commented on a commit
- `plugins.Gogs.format.note-issue` - The format that is used if someone commented on a issue
- `plugins.Gogs.format.note-snippet` - The format that is used if someone commented on a snippet

For those formats you can pass different arguments that contain the values of the notification. The default values are:

- The data of the payload as described
  [here](https://gogs.com/gogs-org/gogs-ce/blob/master/doc/web_hooks/web_hooks.md)
- `project` - The project containing the *name* and the *id* of the project
- `url` - The direct url to the data described by this notification
