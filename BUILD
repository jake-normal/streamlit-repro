python_sources(
    name="root",
)

BASE_IMAGE = "python:3.11.8-slim"

file(name="main", source="main.py")

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
    args=["run", "main.py", "--server.fileWatcherType=none", "--server.port=8080", "--server.headless=true", "--browser.gatherUsageStats=false", "--client.toolbarMode=minimal"],
    dependencies=["main.py:root"],
    execution_mode="venv",
)

docker_image(
    name="image",
    instructions=[
        f"FROM {BASE_IMAGE}",
        "COPY py-bin.pex /bin/app/py-bin.pex",
        "COPY main.py main.py",
        'ENTRYPOINT ["/bin/app/py-bin.pex"]',
        "EXPOSE 8080",
    ],
    dependencies=[":main"],
)
