FROM node:10

# Add a user to run the application
RUN groupadd appuser && \
    useradd -rg appuser appuser

# Create app working directory
WORKDIR /opt/app

COPY . /opt/app

USER root

# Install all the dependencies 
RUN npm install

# Set permission and start application as non-root user
RUN chown -R appuser /opt/app/
USER appuser

EXPOSE 5000
ENTRYPOINT [ "node"]
CMD [ "index.js" ]
