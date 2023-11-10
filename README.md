# guide-Avail-fullnode
Bonjour, avec ce tutoriel, vous pouvez exécuter un nœud complet Avail étape par étape. Vous aurez besoin d'une machine virtuelle Linux.
guide en anglais: https://github.com/0xpatatedouce/step-by-step-availfullnode/edit/main/README.md

1)Configuration de votre environnement:
Installation des dépendances requises :
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install make clang pkg-config libssl-dev build-essential git screen protobuf-compiler -y
```

2)Installation de Rust :
```
curl https://sh.rustup.rs -sSf | sh
```
Sélectionnez 1,
Rust est installé, configurez votre shell actuel :
```
source $HOME/.cargo/env
```

Installez nightly :
```
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

Vérifiez la version de Rust :
```
rustc —version
```

3)Clonez le dépôt du nœud complet :
```
git clone https://github.com/availproject/avail.git
```

4)Installation du client :
(Il existe deux méthodes pour exécuter votre nœud complet, mais je recommande la deuxième pour une meilleure gestion)

Première méthode :
```
cd avail
screen -S avail
```
```
git checkout v1.8.0.0
```
```
cargo build --release -p data-avail
```

![debut 11](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/8d8f5096-adaf-4115-b5e4-829c8c077a21)
![finis 2](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/77dcf3a6-c78b-4590-8249-6b06075af4ae)

```
cargo build --release
```

![debut 1](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/61fc6b92-8091-4258-81fb-9dd1b7c9646b)
![finiiii](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/3c86c379-bc15-48fe-b2b9-87264bcda9b7)

5)Lancez le nœud :
```
./target/release/data-avail --base-path `pwd`/data --chain goldberg --name patatedoucetest
```

Remplacez "patatedouce" par le nom de votre nœud avant d'exécuter votre nœud.
![oui](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/e183e399-6c1e-46f0-9b04-ad616c5d0a35)

Pour vérifier l'état du nœud, consultez la télémétrie d'Avail : http://telemetry.avail.tools/. Votre nœud apparaîtra sur le site sous le nom du "nœud".
Vous devriez voir votre nœud en cours de synchronisation en gris comme dans l'exemple.
![sync](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/d69df11f-54cd-4fe8-854d-a8a654e29311)

Deuxième méthode :
```
mkdir -p output
mkdir -p data
git checkout v1.8.0.0
cargo run --locked --release -- --chain goldberg -d ./output
```
![hh](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/f3a366e3-8e5d-45fd-9a16-98e73f800bf2)

```
sudo touch /etc/systemd/system/availd.service
sudo nano /etc/systemd/system/availd.service
```

Copiez ceci et collez-le dans le fichier, n'oubliez pas de changer le nom "patatedouce" par votre nom:
```
[Unit]
Description=Avail Validator
After=network.target
StartLimitIntervalSec=0
[Service]
User=root
ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain goldberg --name "patatedoucetest"
Restart=always
RestartSec=120
[Install]
WantedBy=multi-user.target
```

![syste](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/807fc945-be3b-43e5-a99c-0ff07d08e2b6)
Ctrl+x press y to save the file and enter to exit

Pour activer votre fichier :
```
sudo systemctl enable availd.service
```

Démarrer le nœud :
```
sudo systemctl start availd.service
```

Vous pouvez vérifier l'état du nœud :
```
sudo systemctl status availd.service
```
![Capture d’écran 2023-10-16 232922](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/b4505035-f6fa-4b54-9bc5-d22819f86018)

Pour regarder les logs du nœud :
```
journalctl -f -u availd
```

![log](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/48b64c7b-46c5-4fba-b7bd-ac1903f0c151)

Pour arrêter le nœud :
```
sudo systemctl stop availd.service
```

Pour vérifier l'état du nœud, consultez la télémétrie d'Avail : http://telemetry.avail.tools/. Votre nœud apparaîtra sur le site sous le nom du "nœud".
Vous devriez voir votre nœud en cours de synchronisation en gris comme dans l'exemple.
![sync](https://github.com/0xpatatedouce/step-by-step-availfullnode/assets/123324096/d69df11f-54cd-4fe8-854d-a8a654e29311)

D'autres guides:
https://github.com/0xrishitripathi/avail-anywhere/tree/main
https://github.com/DinhCongTac221/Install-Avail-Full-Node
https://youtu.be/HYBzK-jJIeQ

Quelques commandes utiles :
Pour quitter la fenêtre ouverte : (Ctrl + a + d)
Pour arrêter le journal/nœud : (Ctrl +c)
Revenir à la fenêtre : screen -x
