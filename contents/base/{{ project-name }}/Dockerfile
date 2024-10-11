FROM python:3.11-buster

RUN pip install poetry

ENV POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

WORKDIR /app

COPY pyproject.toml poetry.lock ./
RUN touch README.md

ADD src /app/src

RUN poetry install --without dev && rm -rf $POETRY_CACHE_DIR

ENV VIRTUAL_ENV=/app/.venv \
    PATH="/app/.venv/bin:$PATH"

COPY logging.yaml ./

{% if "FastAPI" in transport or "gRPC" in transport -%}
EXPOSE 8080
{% endif %}
ENTRYPOINT ["poetry", "run", "python", "-m", "{{ project_name }}.main"]
