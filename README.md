# MasterChef Database

A MySQL database for running a national cooking competition: chefs and their cuisines, recipes with ingredients, equipment and nutritional data, and the competition's episodes with participants, judges and scores. The project covers the full cycle from ER design to the relational schema, indexing, stored procedures, access control and analytical queries.

Developed for the *Databases* course (2024), School of Electrical and Computer Engineering, NTUA.

## Schema

23 tables, including:

| Area | Tables |
|---|---|
| Chefs | `chef`, `specializes`, `cuisine` |
| Recipes | `recipe`, `contains`, `ingredient`, `food_group`, `recipe_calories`, `equipment`, `recipe_equipment`, `label`, `recipe_label`, `meal_type`, `recipe_meal_type`, `them_unit`, `recipe_them_unit` |
| Competition | `episode`, `is_assigned`, `is_graded` |
| Media | `picture` |

The schema also includes:

- **34 indexes** that support the analytical queries.
- **5 stored procedures**:
  - `calculate_winner`: picks an episode's winner from the judges' scores,
  - `calory_count`: computes recipe calories from ingredient quantities,
  - `generate_recipe_them_units`,
  - `update_assigned_recipe`,
  - `update_info`: lets chefs update their own data.
- **Database users with restricted privileges**: an admin account, and per-chef accounts that may only execute the procedures they need.

## Repository layout

```
manual download/
  masterchef_DDL.sql     schema: tables, keys, indexes, stored procedures
  masterchef_DML.sql     sample data
  create_users.sql       database users and privileges
Data Tables/             source data as CSV
queries.sql              analytical queries for the assignment questions (3.1-3.15)
python_codes/
  generate_episode.py    generates competition episodes with random assignments and scores
  DB_PIC_INSERT.py       stores images in the database
  show_picture.py        reads and displays stored images
Αναφορά Εργασίας.pdf     project report (in Greek)
```

## Setup

Requirements: MySQL 8, and Python 3 for the optional scripts.

```bash
git clone https://github.com/harrissfiris/masterchef-db.git
cd masterchef-db
mysql -u root -p
```

```sql
SOURCE manual download/masterchef_DDL.sql;
SOURCE manual download/masterchef_DML.sql;
SOURCE manual download/create_users.sql;
```

Optional Python scripts:

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install mysql-connector-python pillow
python python_codes/generate_episode.py
```

The scripts connect to `localhost` with the `admin` user created by `create_users.sql`.

## Example queries

`queries.sql` answers questions such as:

- average score per chef and per cuisine,
- chefs who never served as judges, and the judges who scored a competitor highest,
- the most frequent pairs of recipe labels in the competition,
- the episode with the most equipment, and the most difficult episode of each year,
- average carbohydrates per year, and food groups never used in a recipe.

Some queries are also given with an alternative execution plan that forces a specific index (`FORCE INDEX`), for comparison.
