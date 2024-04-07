FROM tiangolo/uvicorn-gunicorn-fastapi:python3.10

WORKDIR /app/

# Setup pip mirrors and install pipx
ARG RAW_HOST=https://raw.githubusercontent.com
RUN wget $RAW_HOST/waketzheng/carstino/master/pip_conf.py && python pip_conf.py && pip install -U pip pipx

# Install Poetry
RUN pipx install poetry && \
    cd /usr/local/bin && \
    ([ -f ./poetry ] || ln -s /root/.local/bin/poetry) && \
    python /app/pip_conf.py --poetry && \
    poetry config virtualenvs.create false

# Copy poetry.lock* in case it doesn't exist in the repo
COPY ./pyproject.toml ./poetry.lock* /app/

# Allow installing dev dependencies to run tests
ARG INSTALL_DEV=false
RUN bash -c "if [ $INSTALL_DEV == 'true' ] ; then poetry install --no-root ; else poetry install --no-root --only main ; fi"

ENV PYTHONPATH=/app

COPY ./scripts/ /app/

COPY ./alembic.ini /app/

COPY ./prestart.sh /app/

COPY ./tests-start.sh /app/

COPY ./app /app/app
