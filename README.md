# Court-IQ NBA Player Image Converter

## Project Overview

This repository contains a data transformation script that generates SQL `INSERT` statements for NBA player image links using internal NBA player ID numbers. The project was developed during the early research and prototyping phase of **Court-IQ**, a basketball analytics application aimed at helping users make more informed betting decisions through intuitive data visualization.

At the time, it was unclear whether reliable data sources or APIs would be available. One key challenge was determining how to consistently retrieve NBA player headshots. It was discovered that the NBA hosts official player images at URLs based on internal player IDs. For example:

```
https://cdn.nba.com/headshots/nba/latest/1040x760/1628973.png
```

This script automates the process of extracting these IDs from a public dataset and converting them into usable image links for bulk SQL seeding.

## Background

The NBA player data used by this script comes from a CSV file called `NBA_Player_IDs.csv`, located within the publicly available GitHub repository:

- [NBA Player Team IDs - GitHub](https://github.com/djblechn-su/nba-player-team-ids)

This dataset includes over 4,500 players along with their internal NBA ID numbers, going back nearly a century. Although some more recent players are missing (last commit was in 2020), and not all IDs return valid images (particularly for historical players), it served as a strong foundation.

To use this data with the script:

- Download the `NBA_Player_IDs.csv` file from the source repository above.
- Convert it to JSON using a free [CSV-to-JSON converter](https://csvjson.com/csv2json) or similar tool.
- Save the converted file as `playerImageSource.json` in the root directory of this repo.

## Features

- **Automated SQL Generation**: Outputs SQL `INSERT` statements for player name, birthdate, and image URL.
- **Dynamic Image URL Construction**: Builds image links using internal NBA ID values.
- **String Sanitization**: Cleans player names to avoid SQL syntax issues (e.g., quotes, periods).
- **Duplicate Detection**: Logs duplicate player names to avoid potential conflicts.
- **Extensible**: Can be re-run with new or extended data sources to regenerate SQL files.

## Input Data Format

The script expects an input JSON file (`playerImageSource.json`) with fields like:

```json
{
  "NBAName": "AJ Hammons",
  "NBABirthDate": "8/27/1992",
  "NBAID": "1627773"
}
```

Players without a valid `NBAName` are skipped.

## Output

Running the script will generate:

- `OutputSQLPlayersImage.sql`: Contains SQL insert statements such as:

```sql
('aj hammons', '8/27/1992', 'https://cdn.nba.com/headshots/nba/latest/1040x760/1627773.png'),
```

These statements can then be used to populate a database table within the Court-IQ project or any other basketball-related application.

### Example SQL Table Schema

```sql
CREATE TABLE playersimage (
  player_id SERIAL PRIMARY KEY,
  player TEXT NOT NULL,
  birth_date TEXT,
  image_url TEXT
);
```

## Usage Instructions

1. **Ensure Node.js is installed.**

2. **Clone the repository:**

```bash
git clone https://github.com/jorammercado/nba-player-image-converter.git
cd nba-player-image-converter
```

3. **Run the script:**

```bash
node index.js
```

This will read from the included `playerImageSource.json` and generate:

- `OutputSQLPlayersImage.sql` (in the same directory)

4. **(Optional)**: To use your own data:

- Convert a CSV file (e.g., `NBA_Player_IDs.csv`) to JSON.
- Replace or rename it as `playerImageSource.json` and place it in the root directory.
- Re-run the script as shown above.

## Related Projects

- [Court-IQ Frontend](https://github.com/jorammercado/court-iq)
- [Court-IQ Backend](https://github.com/jorammercado/court-iq-server)

## License

This project is licensed under the [MIT License](https://opensource.org/license/mit).

## Contact

For questions or feedback:

- Joram Mercado — [GitHub](https://github.com/jorammercado) | [LinkedIn](https://www.linkedin.com/in/jorammercado)