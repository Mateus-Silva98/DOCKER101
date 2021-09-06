# Docker Networks

- Bridge: É a rede default do Docker, utilizado para comunicação entre containers.
- Host: Remove o isolamento de rede, o container responde diretamente pela placa de rede do host.
- Overlay: Permite a comunicação entre containers de hosts diferentes.
- Macvlan: Permite atribuir um endereço MAC ao container tornando ele visível como um dispositivo
físico na rede.
- None: Sem rede.

## Network Bridge

Git do projeto `https://github.com/mark-church/docker-pets`

Criamos a rede bridge

```bash
docker network create -d bridge petsBridge
```

Criamos o backend

```bash
docker run -d --net petsBridge --name db consul
```

Criamos o frontend

```bash
docker run -it --env "DB=db" --net petsBridge --name web -p 8000:5000 chrch/docker-pets:1.0
```

## Network Overlay

- TCP port 2377 for cluster management communications

- TCP and UDP port 7946 for communication among nodes

- UDP port 4789 for overlay network traffic

### Escolha um servidor para ser o principal

Iniciando o swarm

```bash
docker swarm init --advertise-addr 192.168.0.28
```

No servidor secundário

Ingresse o servidos ao cluster swarm

```bash
docker swarm join --token SWMTKN-1-2xs8wa75la9ugfaiby0bjtpj71uqi48idxlykvjyft6g45lypo-30uglrk1mhg1yplvptarhp7cv 192.168.0.28:2377
```

Listamos os nodes para verificar se estão funcionais

```bash
docker node ls
```

Criamos a rede overlay

```bash
docker network create -d overlay petsOverlay
```

Criamos nosso backend

```bash
docker service create --network petsOverlay --name db consul
```

Criamos nosso frontend

```bash
docker service create --network petsOverlay -p 8000:5000 -e 'DB=db' --name web chrch/docker-pets:1.0
```

listamos nossos serviços

```bash
docker service ls
```

Escalar a aplicação

```bash
docker service scale web=3
```
