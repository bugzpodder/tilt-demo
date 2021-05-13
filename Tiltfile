allow_k8s_contexts("dev-cat")

load("ext://helm_remote", "helm_remote")

helm_remote("traefik",
  repo_name="traefik",
  repo_url="https://helm.traefik.io/traefik",
  version="9.12.3",
  set=["service.type=ClusterIP"])

k8s_resource("traefik", port_forwards=["8000:8000", "9000:9000"])

docker_build(
  ref="server",
  context="./server",
  dockerfile="./configs/Dockerfile.backend",
  live_update=[
    sync("./server", "/app"),
    run("cd /app && poetry install", trigger=["./server/poetry.lock"]),
  ],
  entrypoint="poetry run uvicorn main:app --reload --port 8001 --host 0.0.0.0",
)

ui_yaml = helm("./configs/charts/ui", name="ui", values=["./configs/charts/ui/values.yaml"])


docker_build(
  ref="ui",
  context="./ui",
  dockerfile="./configs/Dockerfile.frontend",
  live_update=[
    sync("./ui", "/app"),
    run("cd /app && yarn", trigger=["./ui/yarn.lock"]),
  ],
  target="build",
  entrypoint="yarn start",
)

ui_yaml = helm("./configs/charts/ui", name="ui", values=["./configs/charts/ui/values.yaml"])

k8s_yaml(ui_yaml)

server_yaml = helm("./configs/charts/server", name="server", values=["./configs/charts/server/values.yaml"])

k8s_yaml(server_yaml)