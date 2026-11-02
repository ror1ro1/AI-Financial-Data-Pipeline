import os
import sqlite3
import json
from google import genai
from google.genai import types

# Inicjalizacja klienta Gemini API
client = genai.Client()

# Krok 1: Przygotowanie relacyjnej bazy danych i symulacja tabeli finansowej
def init_financial_db():
    conn = sqlite3.connect('company_finance.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS quarterly_revenue (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            quarter TEXT,
            department TEXT,
            revenue_usd INTEGER,
            expenses_usd INTEGER
        )
    ''')
    
    # Czyszczenie i wstawianie testowych danych biznesowych do analizy
    cursor.execute('DELETE FROM quarterly_revenue')
    financial_records = [
        ('Q1', 'Cloud Infrastructure', 520000, 410000),
        ('Q2', 'Cloud Infrastructure', 580000, 430000),
        ('Q3', 'Cloud Infrastructure', 490000, 450000),
        ('Q1', 'AI Services Solution', 120000, 150000),
        ('Q2', 'AI Services Solution', 280000, 190000),
        ('Q3', 'AI Services Solution', 450000, 210000)
    ]
    cursor.executemany('''
        INSERT INTO quarterly_revenue (quarter, department, revenue_usd, expenses_usd)
        VALUES (?, ?, ?, ?)
    ''', financial_records)
    conn.commit()
    return conn

# Krok 2: Ekstrakcja danych przy użyciu agregacji SQL
def fetch_aggregated_data(conn):
    cursor = conn.cursor()
    cursor.execute('''
        SELECT quarter, department, revenue_usd, expenses_usd,
               (revenue_usd - expenses_usd) as net_profit
        FROM quarterly_revenue
    ''')
    rows = cursor.fetchall()
    
    data_summary = []
    for row in rows:
        data_summary.append({
            "quarter": row[0],
            "department": row[1],
            "revenue": row[2],
            "expenses": row[3],
            "net_profit": row[4]
        })
    return data_summary

# Krok 3: Analiza danych przez Agenta AI i generowanie wniosków biznesowych
def generate_ai_financial_report(data_json):
    prompt = f"""
    You are an expert Financial Data Analyst AI. Analyze the following raw quarterly financial data 
    extracted from the SQL database. 
    
    Identify key trends, cross-department growth trajectories, profit margins, operational risks, 
    and provide 3 strategic recommendations for the Executive Board.
    
    Format the output as a professional executive report in Markdown.
    
    Data Source (JSON):
    {json.dumps(data_json, indent=2)}
    """

    response = client.models.generate_content(
        model='gemini-2.5-flash',
        contents=prompt,
        config=types.GenerateContentConfig(
            temperature=0.1
        ),
    )
    return response.text

def main():
    print("Inicjalizacja bazy danych SQL i generowanie struktur...")
    db_conn = init_financial_db()
    
    print("Wykonanie zapytań SQL i agregacja wskaźników finansowych...")
    extracted_data = fetch_aggregated_data(db_conn)
    db_conn.close()
    
    print("Orkiestracja Agenta AI i generowanie raportu biznesowego...")
    markdown_report = generate_ai_financial_report(extracted_data)
    
    # Krok 4: Ładowanie (Zapis) wygenerowanego raportu do pliku zewnętrznego
    output_filename = "Executive_Financial_Report.md"
    with open(output_filename, "w", encoding="utf-8") as file:
        file.write(markdown_report)
        
    print(f"\n[SUKCES] Potok danych zakończony sukcesem. Raport zapisano w: {output_filename}")

if __name__ == "__main__":
    main()
