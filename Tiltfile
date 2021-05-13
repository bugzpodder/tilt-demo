allow_k8s_contexts("dev-cat")

# load("ext://helm_remote", "helm_remote")

docker_build(
  ref="ui",
  context="./ui",
  dockerfile="./configs/Dockerfile.frontend",
  live_update=[
    sync("./ui/", "/app/"),
    run("cd /app && yarn", trigger=["./ui/yarn.lock"]),
  ],
  target="build",
  entrypoint="sleep infinity & wait",
)

ui_yaml = helm("./configs/charts/ui", name="ui", values=["./configs/charts/ui/values.yaml"])

k8s_yaml(ui_yaml)
k8s_resource("ui", port_forwards=["3000:3000"])