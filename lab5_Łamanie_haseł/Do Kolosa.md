Usuwanie wszystkich kontenerów na raz (usuwa działające i zatrzymane)
```
podman rm -f $(podman ps -aq)
```

