# Execute user data after the initial launch of my EC2 instance

By default user data is executed once, at the first boot of the instance. There are situations that would benefit from the ability to execute user data a second time. May be dump some data to logs or recover SSH keys

Follow this article in **[Youtube](https://www.youtube.com/channel/UC_evcfxhjjui5hChhLE08tQ/playlists)**

![Execute user data every boot](https://raw.githubusercontent.com/miztiik/AWS-Demos/master/How-To/setup-user-data-to-execute-after-initial-launch/images/setup-user-data-to-execute-after-initial-launch-00.png)

### Prerequisites
1. EC2 Instance [Get help here](https://www.youtube.com/watch?v=KDtS6BzJo3A)

### Registering a Manual Snapshot Repository
If the instance is running already, stop it and update the `user-data` field with the following code,
```sh
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash
/bin/echo -e "-=-=-=-I execute every time the Server Boots-=-=-=-\n Last execution time:`date`\n" >> /tmp/`date +%Y-%m-%d`
--//
```


Output,
![es-manual-snaps](https://raw.githubusercontent.com/miztiik/AWS-Demos/master/How-To/setup-manual-elasticsearch-snapshots/images/ElasticSearch-Manual-Snapshots-04.png)

##### References
[1] - [AWS Docs - Working with Amazon Elasticsearch Service Index Snapshots](https://aws.amazon.com/premiumsupport/knowledge-center/execute-user-data-ec2/)