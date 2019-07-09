# Sunroc System

This is a docker-compose wrapper for all of the various apps used by Sunroc Rewards.

## Setup

To get all the sites up and running, you'll need to make sure that all of the application folders are siblings to this folder, like this:

```
drwxr-xr-x  19 dan  staff    608 Feb  1 01:36 sunroc_rewards
drwxr-xr-x   8 dan  staff    256 Mar  4 15:29 sunroc_system
drwxr-xr-x  23 dan  staff    736 Feb 28 15:47 sunroc_transactions
```

Then, run the following:

```bash
# Change directories.
cd sunroc_system

# Make a .env file for the docker containers.
cp .env.sample .env

# Get dependencies.
docker-compose run --rm rewards mix deps.get
docker-compose run --rm transactions mix deps.get

# Build docker images anew (this is especially important for
# sunroc_transactions because of the Rewards_Data_View.)
docker-compose build

# Make sure all the environment variables are correct, and then, start the
# containers, starting with the mssql container:
docker-compose up -d mssql

# Wait for a good 2 minutes before starting everything else up.
sleep 120 && docker-compose up -d

# Follow the logs to make sure everything is running:
docker-compose logs -f

# Check containers with the following:
docker-compose ps
```

You should now be able to access the different applications via their open ports:

* http://localhost:4000/ - The rewards app.
* http://localhost:4001/ - The invoice processor app's dashboard.
* http://localhost:4042/ - The development mail inbox (may not be implemented yet).
