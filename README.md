# Global Energy Consumption & Emissions Database

## Overview

This project is a comprehensive SQL-based analytical platform designed to track and analyze global energy consumption, production, emissions, and economic indicators across countries and years. It provides deep insights into energy trends, carbon footprints, GDP correlations, and population dynamics to support climate research, policy development, and sustainability initiatives.

## Features

- **Multi-Table Database Structure**: Organized data across 6 interconnected tables
- **Comprehensive Analysis**: 12+ pre-built SQL queries for various analytical perspectives
- **Trend Analysis**: Year-over-year emission changes, energy consumption patterns, and GDP growth
- **Comparative Metrics**: Energy consumption per capita, emissions by country, and energy efficiency ratios
- **Sustainability Insights**: Carbon footprint tracking, energy type contributions, and per-capita emission analysis
- **Policy Support**: Data-driven insights for climate and energy policy development

## Database Schema

### Tables

1. **country**
   - Stores country names and unique identifiers
   - Primary key: `country`

2. **consumption**
   - Energy consumption data by country, energy type, and year
   - Fields: country, energy, year, consumption

3. **production**
   - Energy production data by country, energy type, and year
   - Fields: country, energy, year, production

4. **emission**
   - Carbon emissions data including per-capita metrics
   - Fields: country, energy_type, year, emission, per_capita_emission

5. **gdp_ppp**
   - Gross Domestic Product (PPP) by country and year
   - Fields: country, year, value

6. **population**
   - Population statistics by country and year
   - Fields: country, year, value

## Key Queries

### General & Comparative Analysis
- **Q1**: Total emissions per country for the most recent year
- **Q2**: Top 5 countries by GDP in the most recent year
- **Q3**: Energy types with highest emission contributions

### Trend Analysis Over Time
- **Q4**: Global year-over-year emission changes
- **Q5**: GDP trends for each country
- **Q6**: Population growth impact on emissions
- **Q7**: Energy consumption trends for major economies
- **Q8**: Average yearly per-capita emission changes
- **Q9**: Energy consumption per capita over the last decade
- **Q10**: Countries with highest energy consumption relative to GDP

### Global Comparisons
- **Q11**: Top 10 countries by population and their emissions
- **Q12**: Global emission share by country (percentage)

## Installation & Setup

### Prerequisites
- MySQL Server 5.7 or higher
- SQL client (MySQL Workbench, Command Line, or any IDE)

### Steps

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd energy-database
   ```

2. Create the database and tables:
   ```bash
   mysql -u [username] -p < energy_db.sql
   ```

3. Verify database creation:
   ```bash
   mysql -u [username] -p ENERGY
   SELECT * FROM country;
   ```

## Usage

### Running Queries

Connect to the ENERGY database and execute any of the pre-built queries:

```sql
-- Example: Get total emissions per country
SELECT country, SUM(emission) as Total_emission
FROM emission
WHERE year = (SELECT MAX(year) FROM emission)
GROUP BY country
ORDER BY Total_emission DESC;
```

### Data Import

If you have raw CSV data, import it using:

```sql
LOAD DATA LOCAL INFILE '/path/to/file.csv'
INTO TABLE table_name
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n';
```

## Analysis Examples

### Energy Efficiency Analysis
Find countries with highest energy consumption relative to GDP:
```sql
SELECT country, 
       ROUND(SUM(consumption)/SUM(gdp), 5) as efficiency_ratio
FROM consumption c 
JOIN gdp_ppp g ON c.country = g.country
GROUP BY country
ORDER BY efficiency_ratio DESC;
```

### Carbon Footprint Tracking
Monitor per-capita emissions trends:
```sql
SELECT country, year, 
       ROUND(AVG(per_capita_emission), 6) as avg_per_capita
FROM emission
GROUP BY country, year
ORDER BY country, year;
```

### Economic-Environmental Correlation
Analyze GDP and emission relationships:
```sql
SELECT e.country, e.year, 
       ROUND(SUM(e.emission)/g.value, 5) as emission_to_gdp
FROM emission e 
JOIN gdp_ppp g ON e.country = g.country 
  AND e.year = g.year
GROUP BY country, year
ORDER BY country, year;
```

## Data Insights

This database enables analysis of:

- Energy consumption patterns across different regions and time periods
- Emission trends and their correlation with economic growth
- Per-capita energy usage and carbon footprints
- Renewable vs. fossil fuel contributions
- Population growth impact on energy demand
- Energy efficiency metrics by country
- Policy effectiveness in reducing emissions

## Use Cases

- **Climate Research**: Track global emission trends and identify high-emission countries
- **Policy Development**: Data-driven insights for energy and climate policies
- **Sustainability Reporting**: Corporate and national carbon footprint analysis
- **Investment Analysis**: Identify energy efficiency opportunities and green investment targets
- **Academic Research**: Comprehensive dataset for energy economics and environmental studies

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contact & Support

For questions, issues, or suggestions, please open an issue on GitHub or contact the maintainers.

## Acknowledgments

- Data sourced from global energy databases and international climate organizations
- Built to support sustainable development and climate action research
- Designed for accessibility and ease of use in both academic and professional settings

## Disclaimer

This database is provided for educational and research purposes. Users should verify data accuracy and source credibility before using insights for critical decision-making. Always consult official energy and climate organizations for authoritative data.

---

**Last Updated**: December 2024  
**Version**: 1.0.0
