## Przygotowanie systemu

#Instalacja pakietów pomocniczych (narzędzia instalacji z zewnętrznych repo, python3 i git)

sudo apt install apt-transport-https ca-certificates curl software-properties-common
sudo apt install git python3 python3-pip 

#Dodanie klucza publicznego repozytorium Docker Engine
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

#Dodanie repozytorium Docker Engine
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#Aktualizacja chache pakietów, następnie instalacja docker-ce i docker-compose
sudo apt update
sudo apt install docker-ce docker-compose


#Dodanie bieżącego użytkownika do grupy docker, aby miał on pełne uprawnienia do kontenerów.
sudo usermod -aG docker $USER
su - ${USER}

#Sprawdzamy czy docker jest działający i gotowy do uruchomienia WebODM
sudo systemctl status docker


## Instalacja WebODM

## Krok 1 - pobieramy z repo github (tylko raz)
git clone https://github.com/OpenDroneMap/WebODM --config core.autocrlf=input --depth 1
cd WebODM

# Krok 2 - startujemy kontenery, pobiorą się i zaktualizują. Ta operacja każdy raz po restarcie serwera.
./webodm.sh start

# Krok 2a - aktualizacja
./webodm.sh stop
./weodm.sh update

# Krok 3 - certyfikat SSL (opcjonalny)
Konieczne jest wcześniejsze dodanie rekordu DNS dla wskazanego IP, następnie

./webodm.sh restart --ssl --hostname adreswebodm.polsl.pl
