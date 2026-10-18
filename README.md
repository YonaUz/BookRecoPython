# BookRecoPython

A smart, first trial data-driven "bundle shopping" pipeline that bridges machine learning recommendation systems with an automated asynchronous Vinted scraper. This project identifies books tailored to your reading profile and automatically locates European Vinted sellers ("Vinties") stocking multiple titles from your wishlist to optimize bundle shipping costs. And currently under construction to combine the 2 features together. 

---

## Repository Structure

The repository is organized into four core notebooks covering exploratory tests, machine learning models, and automated web scraping:

1. **`1_VintedScraper_Synch_Asynch.ipynb`**
   * Initial experiments comparing synchronous and asynchronous approaches for querying the Vinted API.
   * Testing pagination and basic error handling.

2. **`2_VintedScraper_Asynch.ipynb`**
   * Production-ready asynchronous Vinted scraper (`vinted-scraper`).
   * Targets the European pool (France, Belgium, Spain, Italy, Netherlands, Portugal, Luxembourg).
   * Implements anti-bot strategies (sequential pacing, randomized delays, and rate-limiting) to mitigate HTTP `403 Forbidden` errors and JSON parsing issues.

3. **`3_BooksCosineSimilarity.ipynb`**
   * **Collaborative Filtering Engine**: Recommends books based on how users rated them.
   * Utilizes the Amazon Books Review dataset.
   * Employs memory-efficient sparse matrices (`scipy.sparse.csr_matrix`) and `float32` optimization to handle millions of ratings without hitting `MemoryError` limits.
   * Computes **Cosine Similarity** across user rating patterns.

4. **`4_BooksRecoReviewCosineSimilarity.ipynb`**
   * **Content-Based Filtering Engine**: Recommends books based on semantic text analysis of reviews.
   * Aggregates textual reviews per title and transforms them into numerical vectors using **TF-IDF Vectorization** (`TfidfVectorizer`).
   * Computes **Cosine Similarity** to match books sharing a similar lexical field, themes, and vocabulary.

---

## Tech Stack

* **Language**: Python 3.12+
* **Data Science & ML**: `pandas`, `numpy`, `scipy`, `scikit-learn`
* **Web Scraping**: `vinted-scraper`, `asyncio`, `aiohttp`
* **Visualizations**: `matplotlib`, `seaborn`

---

## How It Works (The End-to-End Workflow)

1. **Recommendation Phase**: Run notebook `3` or `4` to generate a curated list of book recommendations starting from a target title.
2. **Integration Phase**: Pass the recommended book list into the asynchronous Vinted scraper configuration (`2_VintedScraper_Asynch.ipynb`).
3. **Scraping Phase**: The scraper queries Vinted across multiple European domains while respecting anti-bot constraints.
4. **Matching Phase**: The script aggregates listings by seller, identifying "Vinties" who stock **two or more books** from your wishlist, allowing you to buy a cost-effective bundle.

---

## Installation & Setup

Clone the repository and install the required dependencies:

```bash
git clone [https://github.com/your-username/BookRecoPython.git](https://github.com/your-username/BookRecoPython.git)
cd BookRecoPython
pip install pandas numpy scipy scikit-learn matplotlib seaborn vinted-scraper
