# Diesel Object Relation Mapper

Diesel provides a separate CLI tool to help manage your project. Since it's a standalone binary, and doesn't affect your project's code directly, it should be installed on your system separately.

```bash
sudo apt install libpq-dev
cargo install diesel_cli --no-default-features --features postgres
```

To setup the database for the Diesel CLI create a environment file called `.env`:

    DATABASE_URL=postgres://username:password@localhost/diesel_demo

After that run `diesel setup` which will setup everything. It will create the database if not already there and creates a migrations directory for you.


{!docs/abbreviations.txt!}

