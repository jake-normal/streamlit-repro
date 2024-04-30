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
)

docker_image(
    name="image",
    instructions=[
        f"FROM {BASE_IMAGE}",
        "COPY py-bin.pex /bin/app/py-bin.pex",
        'ENTRYPOINT ["/bin/app/py-bin.pex"]',
        "EXPOSE 8080",
    ],
)
