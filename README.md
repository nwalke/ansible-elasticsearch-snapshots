Elasticsearch Snapshots
=========

The purpose of this role is to install the necessary AWS plugin to get snapshots on S3 from Elasticsearch.

Requirements
------------

Currently this role assumes that Elasticsearch is running as `elasticsearch` the user and `elasticsearch` the group.

Role Variables
--------------

| Name | Description | Default |
|:-:|:-:|:-:|
| es_snapshot_share_directory | Path to the Elasticsearch `usr/share` directory. | `/usr/share/elasticsearch` |
| es_snapshot_plugin_binary_path | Path to the plugin binary.  Normally this can be safely inferred from the `usr/share` directory. | `{{ es_snapshot_share_directory }}/bin/plugin` |
| es_snapshot_snapshot_script_path | Path to save the snapshot script.  Normally this can be safely inferred from the `usr/share` directory. | `{{ es_snapshot_share_directory }}/bin/snapshot.sh` |
| es_snapshot_plugin_name | Plugin name we want to install.  This changes with Elasticsearch 5.x. | `cloud-aws` |
| es_snapshot_include_file | Include file we want to use.  This gets set as `ES_INCLUDE` before running the plugin binary. | `/etc/default/elasticsearch` |
| es_snapshot_elasticsearch_address | Address to contact Elasticsearch on. | `127.0.0.1` |
| es_snapshot_elasticsearch_rest_port | Port to contact Elasticsearch on. | `9200` |
| es_snapshot_repository_name | Snapshot repository name to use. | `my_s3_repository` |
| es_snapshot_bucket_name | S3 bucket to use. | `my_s3_bucket` |
| es_snapshot_bucket_region | S3 bucket region.  This is used to infer the S3 endpoint to use. | `us-east-1` |
| es_snapshot_plugins_directory | Directory where plugins get installed.  This is used for checking if the plugin has been installed.  Normally this can be safely inferred from the `usr/share` directory. | `{{ es_snapshot_share_directory }}/plugins` |
| es_snapshot_cron_time | Special cron time word to run snapshot script.  Some options include daily, hourly, weekly, monthly, yearly. | `daily` |
| schedule_es_snapshots | Whether or not to schedule the snapshot script.  If you want to take snapshots, set how often via `es_snapshot_cron_time` and set this to `True` | `False` |

Dependencies
------------

This tries to use the `restart elasticsearch` handler available from the `ansible-elasticsearch` role.  You can get it by adding this to your `requirements.yml`:
```
- name: 'ansible-elasticsearch'
  src: 'https://github.com/elastic/ansible-elasticsearch.git'
  version: '0.1.2'
```

Example Playbook
----------------

Make sure this is in your `requirements.yml`:
```
- src: git@github.com:nwalke/ansible-elasticsearch-snapshots.git
  scm: git
  name: elasticsearch-snapshots
```

To use it in your playbook, do this:
```
- hosts: elasticsearch_server
  roles:
     - role: elasticsearch-snapshots
```
License
-------

MIT / BSD
