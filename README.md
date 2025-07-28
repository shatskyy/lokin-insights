# Lokin Insights - Startup Landscape Insight Tool

A comprehensive tool for ingesting, analyzing, and visualizing startup ecosystem data from multiple public sources including AngelList, Product Hunt, and Y Combinator datasets.

## 🎯 Project Goals

- **Automated Data Ingestion**: Daily collection from multiple startup databases
- **Trend Analysis**: Industry trends, tag popularity, and launch patterns
- **Insight Dashboard**: Real-time visualization of startup landscape
- **Idea Generator**: AI-powered startup idea suggestions based on market trends

## 📁 Project Structure

lokin-insights/
├── ingest/ # Data ingestion pipeline
│ ├── configs/ # Source configuration files
│ ├── raw_samples/ # Sample data files
│ └── scripts/ # Ingestion scripts
├── schemas/ # Database schemas and normalization rules
├── src/ # Main application code
│ ├── normalization/ # Data cleaning and standardization
│ ├── analysis/ # Trend analysis and reporting
│ └── dashboard/ # Web interface
├── reports/ # Generated reports and visualizations
│ ├── png/ # Chart images
│ ├── csv/ # Data exports
│ └── json/ # API-ready data
├── docs/ # Documentation
├── logs/ # Application logs
└── tests/ # Unit and integration tests

## 🛠️ Setup Instructions

### Prerequisites

- Python 3.9 or higher
- PostgreSQL 12+ (local or Docker)
- Git

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/YOUR_USERNAME/lokin-insights.git
   cd lokin-insights

   ```
