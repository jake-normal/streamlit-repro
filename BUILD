python_sources(
    name="root",
)

BASE_IMAGE = "python:3.11.8-slim"

pex_binary(
    name="dev",
    script="streamlit",
    args=["run", "main.py"],
    dependencies=["main.py:root"],
    execution_mode="venv",
)


pex_binary(
    name="py-bin",
    complete_platforms=["3rdparty/platforms:docker_python_3_11_8_x86_64"],
    script="streamlit",
    args=["run", "main.py"],
    dependencies=["main.py:root"],
    execution_mode="venv",
    layout="packed",
    include_tools=True,
)

docker_image(
    name="image",
    instructions=[
        f"FROM {BASE_IMAGE} as deps",
        "COPY py-bin.pex /py-bin.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python3.11 /py-bin.pex venv --scope=deps --compile /bin/app",

        f"FROM {BASE_IMAGE} as srcs",
        "COPY py-bin.pex /py-bin.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python3.11 /py-bin.pex venv --scope=srcs --compile /bin/app",

        f"FROM {BASE_IMAGE}",

        'ENTRYPOINT ["/bin/app/pex"]',
        "COPY --from=deps /bin/app /bin/app",
        "COPY --from=srcs /bin/app /bin/app",

        "EXPOSE 8080",
    ],
)
