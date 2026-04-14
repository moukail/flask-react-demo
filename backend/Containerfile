FROM registry.access.redhat.com/ubi9/python-312:9.7

WORKDIR /var/www/app

COPY --chown=1001:0 requirements.txt .
RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt

COPY --chown=1001:0 . .

#RUN chmod -R g=u .

EXPOSE 8080
STOPSIGNAL SIGTERM

USER 1001

CMD ["gunicorn", "app:app", "-b", ":8080"]