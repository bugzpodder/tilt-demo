allow_k8s_contexts('dev-cat')

docker_build(
  ref="ui",
  context="../",
  dockerfile="./configs/Dockerfile.frontend",
  live_update=[
    sync("./ui", "/app"),
    run("cd /app && yarn", trigger=["./ui/yarn.lock"]),
  ],
  target="build",
  entrypoint="yarn dev",
)
