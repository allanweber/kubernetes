sudo nano /etc/nginx/sites-available/allandevtools.com

sudo ln -s /etc/nginx/sites-available/allandevtools.com /etc/nginx/sites-enabled/allandevtools.com

sudo nano /etc/elasticsearch/elasticsearch.yml
discovery.type: single-node

journalctl -u elasticsearch
journalctl -u kibana
journalctl -u logstash


sudo nano /etc/logstash/conf.d/02-logstash.conf
sudo tail -f /var/log/logstash/logstash-plain.log


netstat -nl
sudo ufw status verbose

sudo ufw allow from 82.217.122.125 to any port 5000 proto tcp