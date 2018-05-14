# Help Scout exporter

This small program lets you **download your history of [Help Scout](https://helpscout.net) email threads & conversations**, either for backup purposes or because you are exporting your data archive to another platform. Help Scout is an _email-based customer support / help desk service_.

This program, written in PHP, uses Help Scout's [Mailbox API 1.0](https://developer.helpscout.com/help-desk-api/) to: 
- get your mailboxes list
- get the conversations into those mailboxes 
- get the email threads into those conversations

Exported data will be automatically written into JSON files under the supplied directory path:
- `mailboxes.json` file will list mailboxes metadata.
- `conversations_{mailbox.id}.json` files will list conversations and threads under these mailboxes.

Running this program might be slow if you have lots of conversations as it respects Help Scout API rate-limit of (only) 200 requests per minute. It is not possible to pause/restart the program as it will start all over again when you run it again later, creating potential duplicates in the output files. If the program stopped, you should clean previous files (eg: `rm -R ~/helpscout-data/*.json`) before restarting.

## Requirements

- **PHP 5.6+** for the command line (eg: `sudo apt-get install php-cli`)
- **Composer** the dependency manager for PHP ([documentation](https://getcomposer.org/download/))

## Usage

### 1. Install the script with:

```bash
composer config -g repositories.helpscout-exporter vcs https://github.com/guenard/helpscout-exporter.git
composer require -g guenard/helpscout-exporter
```

**Notes:**

_Add Composer global binaries to your path with `export PATH=$HOME/.composer/vendor/bin:$PATH` (add this line to your `~/.profile` or ` ~/.bashrc`)._

### 2. Configure Help Scout API key:

In your terminal, set the [Help Scout API key](https://docs.helpscout.net/article/1074-manage-api-keys) as an environment variable with:

```bash
export HELPSCOUT_API_KEY=YOUR_API_KEY_HERE
```

### 3. Run the exporter script:

```bash
helpscout-exporter -o ~/helpscout-data/ > ~/helpscout-export.log &
```

**Notes:**

_To let the program run for a very long time a) prefix the command with `nohup` (will detach the process from your current terminal) and b) suffix the command with `&` (will move it in background)._

### 4. Monitor the running exporter script:

```bash
# monitor output log in real-time
tail -f ~/helpscout-export.log
# number of conversations exported
cat ~/helpscout-data/conversations_*.json |grep '"threads":' |wc -l
```

## Disclaimer

Help Scout (R) is a registered Trademark of Help Scout Inc.

This open-source script is *not* affiliated with Help Scout Inc.

Before using this script, please review Help Scout's [Terms of Service](https://www.helpscout.net/company/legal/terms-of-service/).
