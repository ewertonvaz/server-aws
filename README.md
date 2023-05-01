# Experiencias no Laravel 10
## Deploy na AWS

```
aws cloudformation deploy --stack-name laracasts --template-file $PWD/stack.json
aws cloudformation describe-stack-events --stack-name laracasts
aws cloudformation delete-stack --stack-name laracasts
```

https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html

```
aws ec2 describe-instances
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,Type:InstanceType,AZ:Placement.AvailabilityZone,IP_priv:PrivateIpAddress,IP_pub:PublicIpAddress,state:State.Name,key:KeyName }'
aws ec2 stop-instances --instance-ids <value>
aws ec2 start-instances --instance-ids <value>
```

### Instalar PHP/NGINX/Composer
```
sudo apt update
sudo apt install nginx php8.1 php8.1-fpm
sudo apt install zip unzip php8.1-zip php8.1-curl php8.1-xml
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
sudo mv composer.phar /usr/local/bin/composer
```

### Deploy Laravel app
```
ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
cat ~/.ssh/id_rsa.pub
sudo chown ubuntu:ubuntu /var/www
cd /var/www
git clone https://github.com/ewertonvaz/server-aws
cd server-aws
composer install
cp .env.example .env
php artisan key:generate
sudo chown -R www-data:www-data /var/www/server-aws
```

### Configurar NGINX
```
sudo nano /etc/nginx/sites-available/server-aws.conf
sudo ln -s /etc/nginx/sites-available/server-aws.conf /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx reload
```

### Criar usuário para o site:
```
sudo adduser server-aws
sudo mv /var/www/server-aws /home/server-aws/www
sudo chown -R server-aws:server-aws /home/server-aws/www
sudo usermod -a -G server-aws www-data
sudo nano /etc/php/8.1/fpm/pool.d/www.conf
	[www] -> [server-aws]
	user = server-aws
	group = server-aws

	listen.owner = server-aws
	listen.group = server-aws
sudo nano /etc/nginx/sites-available/server-aws.conf
sudo service nginx reload
sudo service php8.1-fpm restart
sudo mkdir /home/server-aws/.ssh
sudo mv /home/ubuntu/.ssh/id* /home/server-aws/.ssh
sudo chown -R server-aws:server-aws /home/server-aws/.ssh
su server-aws
cd /home/server-aws/www
git pull origin main
```
