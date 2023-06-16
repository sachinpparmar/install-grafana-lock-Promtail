# install-grafana-lock-Promtail

# Video-link:     https://youtu.be/QwGm5m4AxNA

# Install Grafana on Debian or Ubuntu
# need to open port no 3000 for grafana ,3100 for loki,

sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key

Stable release
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
Beta release
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
# Update the list of available packages
sudo apt-get update

# Install the latest OSS release:
sudo apt-get install grafana



# Install Loki and Promtail using Docker

# Download Loki Config


wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml

# Run Loki Docker container

docker run -d --name loki -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml

# Download Promtail Config

wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml

# Run Promtail Docker container
sudo apt install docker.io
docker run -d --name promtail -v $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml

# check public_ip:3000 for grafana
#  check public_ip:3100 and also public_ip:3100/ready      for loki     ( make sure port is open )
# if you want to check and inform log to grafana for this aad datastore on grafana page (add your 1st data source)
# choose loki and add url of loki     http://public_ip:3100/      or  http://localhost:3100/ 
# if you want to edit something so go on promtail you can add new target here  for using path (pwd) and copy also in promtail in target path   and restart the promtail container
docker restart container_id
## install nginx for checking more logs  
apt install nginx


