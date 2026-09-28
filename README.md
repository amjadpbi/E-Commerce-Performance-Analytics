# E-Commerce Performance Analytics

Power BI sales analytics built around a synthetic e-commerce dataset and a star-schema-style reporting model.

## Project Overview
This project analyzes product, customer, campaign, channel, payment, and regional sales performance using a Power BI semantic model and report pages. The project is structured as a learning-focused sales analytics exercise using local CSV files.

## Business Context
This is a self-initiated portfolio project using a local e-commerce dataset. The data is instructional and analytical rather than tied to a production system or live business environment.

## Problem
The project needed a compact but realistic e-commerce model to analyze sales performance trends, product behavior, customer segmentation, and channel/campaign effectiveness.

## Solution
The repository contains a PBIP project, PBIX artifact, report definitions, and a semantic model built around a central fact table and multiple dimensions. The model supports revenue, order, customer, campaign, and channel analysis through measured business logic.

## Data
- Source type: CSV fact and dimension files
- Data nature: synthetic/local instructional dataset
- Core tables: fact_sales, dim_customer, dim_product, dim_region, dim_channel, dim_payment, dim_campaign, dim_date
- Evidence: model and report files are present in the project folder

## Technical Approach
- load source CSV files
- define relationships between fact and dimension tables
- build semantic model with sales measures and KPI logic
- present report pages for trend, product, and segment analysis

## Key Analytical Areas
- revenue and order performance
- customer segmentation
- product and regional trends
- campaign and channel performance
- time-based comparison analysis

## Evidence / Scope
The repository contains the PBIP project, PBIX file, report definitions, semantic model, and source data. The project is best described as a synthetic self-initiated analytics implementation rather than a real operational deployment.

## Limitations
- local sample dataset only
- no production deployment evidence
- learning-oriented scope

## Repository Structure
- Ecomm Analysis.pbip — Power BI project
- Ecomm Analysis.Report — report definition files
- Ecomm Analysis.SemanticModel — semantic model definition files
- dim_*.csv and fact_sales.csv — source data
- PROJECT_AUDIT.md — evidence summary

## Tools & Technologies
- Power BI
- CSV source data
- DAX measures
- PBIP/PBIR semantic model

## Project Status
Self-initiated analytical project using synthetic/local data. No live business system or production deployment is represented in the project evidence.
