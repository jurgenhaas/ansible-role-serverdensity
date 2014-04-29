Ansible role to install and configure Server Density Agent
==========================================================

[Server Density] is a monitoring solution which requires a simple Python based agent and is highly configurable. This Ansible role installs and confgures that agent and supports several options like plugin installation and inventory synchronisation with your Server density dashboard.

Currently this plugin is developed and tested for Ubuntu only and will be enhanced to other Nix's later on.

##Dependency##

The [Ansible-ServerDensity-Plugin] is required and installation and instructions can be found in that project.

##Configuration##

In defaults/main.yml you'll find a set of variables that you could re-define in your own inventory. Please, do not overwrite the variables in defaults/main.yml, instead you should define them in your inventory files, e.g. group_vars/all

###sd_url###

Default: ```''```

Defines the Server Density URL of your account, e.g. ```'https://myaccount.serverdensity.io'```

###sd_api_token###

Default: ```''```

Defines your API token provided by Server Density. You get this token by going to preferences in your SD account to the "Security" tab where you can either grab one of the existing API tokens or createing a new one.

###sd_api_cache_file###

Default: ```false```

Each time you're using the SD plugin it reads the current settings for devices, services and alerts from your SD account by using the Server Density API. As this usually takes a couple of seconds it is possible to shortcut that process by storing all those settings in a cache file of your own choice so that next time those values get loaded from that cache file instead through the API.

The plugin makes sure that the cache gets reset if necessary, e.g. if the settings of your inventory have changed.

###sd_agent_key###

Default: ```''```

Do *not* redefine this variable, it is just there to avoid error messages in case a variable was missing. The agent key for each of your hosts gets assigned by Server Density and ill be imported by the plugin and will be used for updating your device settings.

###sd_logging_level###

Default: ```'info'```

Defines the level of log information that the SD agent will produce during run time. Possible values are debug, info, error, warn, warning, critical, fatal

###sd_plugins###

Default: ```[]```

Defines a list of Server Density plugins that will be installed and updated by this role automatically.

Example:

```
  sd_plugins:
    updatestatus: 'UpdateStatus.py'
    redis: 'Redis.py'
```

This variable is expected to be defined as a set so that hash_behaviour=merge can be used so that you can define this variable in several of your groups and that this role is going to install the superset of all the group a specific host belings to.

The key of each entry should be unique across your inventory and the value is a file name. Those files have to be present in a subdirectory called ```files/sd-plugins``` of your inventory directory.

###sd_groups###

Default:

```
  apache: 'none'
  mysql: 'none'
  proxy: 'none'
```

Certain sections in the agent configuration are only relevant for certain hosts and by providing valid inventory group names for those sections (currently apache, mysql, proxy) you can make sure that for all hosts in those groups the relevant parts in the agent configuration get written to that host.

##Detailed configuration for inventory synchronisation##

By defining further variables in your inventory, you get full control over how your inventory gets synchronized with your Server Density dashboard including device groups, services and alerts.

All those options are documented over in the [Ansible-ServerDensity-Plugin]

[Server Density]: https://www.serverdensity.com
[Ansible-ServerDensity-Plugin]: https://github.com/jurgenhaas/ansible-plugin-serverdensity
