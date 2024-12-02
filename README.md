# Nextcloud-on-Ubuntu
Festplatte mount:

1.lsblk
2.sudo fdisk /dev/sda
   p - anzeigen von bestehenden Partitionen
   d - Partition löschen (Vorsicht!!!)
   n - neue Partition anlegen
   p nach Eingabe von n bedeutet primary Partition anlegen
   w - Änderungen speichern
3.sudo mkfs -t ext4 /dev/sda1
4.sudo blkid /dev/sda1
5.sudo mkdir /media/nextcloud
6.sudo nano /etc/fstab
7.UUID=2c734719-21fc-4ee6-8fa6-c243cfb991af /media/nextcloud ext4 defaults 0 0
8.sudo mount -a
9.df -h
10.sudo mkdir /media/nextcloud/data

Docker install:
1.curl -fsSL https://get.docker.com -o get-docker.sh | sudo sh get-docker.sh
2.sudo groupadd docker | sudo usermod -aG docker $USER
3.sudo usermod -aG docker $USER
4.echo $USER
5.sudo nano docker-compose.yml
6.sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose && docker-compose --version

7.sudo docker-compose up -d





services:
  db:
    image: mariadb:10.6
    restart: always
    command: --transaction-isolation=READ-COMMITTED --log-bin=binlog --binlog-format=ROW
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=nc123!
      - MYSQL_PASSWORD=nc123!
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nc

  app:
    image: nextcloud
    restart: always
    ports:
      - 8080:80
    links:
      - db
    volumes:
      - /media/nextcloud/data:/var/www/html
    environment:
      - MYSQL_PASSWORD=nc123!
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nc
      - MYSQL_HOST=db

volumes:
  db:
