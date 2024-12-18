# project-kubernetes

## ROADMAP 
  fleetman-mongodb:
    image: mongo:3.6.23
    ports:
      - 27017:27017
    volumes:
      - mongo-data:/data/db
    networks:
      - fleetman
 
networks:
  fleetman
 
volumes:
  mongo-data

