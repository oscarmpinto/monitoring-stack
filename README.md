# Monitoring Stack

Stack para monitorização de containers, VMs, infra física, Proxmox, Raspberry Pi e network devices usando Prometheus + Grafana.

## Conteúdo
- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- cAdvisor

## Requisitos
- Docker
- Docker Compose
- Ubuntu 22.04+ (ou outra distro Linux com Docker)

## Quick start

1. Clone o repositório.
2. Copie o ficheiro de exemplo para o ambiente:

```bash
cp .env.example .env
```

3. Inicie a stack:

```bash
docker compose up -d
```

4. Abra:
- Grafana: http://localhost:3000
- Prometheus: http://localhost:9090
- Alertmanager: http://localhost:9093

Credenciais padrão do Grafana:
- utilizador: admin
- password: admin

## Estrutura recomendada para NFS

Se quiseres poupar espaço na VM do Docker, podes guardar a data dos serviços no NFS.

Exemplo de montagem:

```bash
sudo mkdir -p /mnt/nfs/monitoring
sudo mount -t nfs 192.168.1.10:/export/monitoring /mnt/nfs/monitoring
```

Depois, podes adaptar os diretórios por volume da stack para apontarem para `/mnt/nfs/monitoring/...`.

## NFS no Docker Compose

Exemplo:

```yaml
volumes:
  - /mnt/nfs/monitoring/prometheus:/prometheus
  - /mnt/nfs/monitoring/grafana:/var/lib/grafana
```

## Monitorização de infra adicional

### Proxmox
Usa a API do Proxmox ou um exporter específico do ecossistema (por exemplo, `proxmox_exporter`), e adiciona um `job` em `prometheus/prometheus.yml`.

### Raspberry Pi
O `node-exporter` já recolhe métricas de CPU, memória, rede e disco do host.

### Switch / SNMP
Para switches, normalmente usa o `snmp-exporter` com SNMP v2c/v3 e targets no Prometheus.

## Useful commands

```bash
docker compose ps
docker compose logs -f
docker compose down
```

## Observações

- Em produção, muda a password do Grafana e ajusta as regras de alertas.
- Para uso real em produção, usa autenticação e nomes de hosts consistentes.
- Se o teu NFS for lento, considera guardar os dados locais ou em SSD para melhor performance.
