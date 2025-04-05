# AI-Driven Personalized Airbnb Recommendation System

## Overview

This project implements an AI-driven system designed to provide personalized Airbnb recommendations. The system addresses the increasing demand for tailored travel experiences by leveraging AI agents to analyze seasonal trends, user behavior, and individual preferences [[1]] [[4]] [[6]]. This approach overcomes the limitations of traditional search methods, offering users more relevant and satisfying travel options [[1]].

## Architecture

The system comprises three core AI agents, each powered by the Mistral-7B-Instruct-v0.1 Large Language Model (LLM). These agents work collaboratively to generate dynamic and personalized recommendations.

### 1. Seasonal Demand & Pricing Agent

*   **Responsibility:** Gathers real-time seasonal demand and pricing data for specified locations.
*   **Implementation:**
    *   Utilizes the Google Trends API to retrieve location-specific seasonal demand metrics.
    *   *(Future Development: Integration with pricing APIs from Airbnb or similar platforms to fetch real-time pricing data.)*
*   **Code Snippet (Conceptual):**

    ```python
    # Example using Google Trends API (Conceptual)
    from pytrends.request import TrendReq

    pytrends = TrendReq(hl='en-US', tz=360)
    kw_list = ["Airbnb in <location>"] # e.g., "Airbnb in Paris"
    pytrends.build_payload(kw_list, cat=0, timeframe='today 12-m', geo='<location_code>') # e.g., geo='FR' for France
    trend_data = pytrends.interest_over_time()
    print(trend_data)
    ```

### 2. User Behavior & Booking History Agent

*   **Responsibility:** Extracts and analyzes user-specific data to understand preferences and booking patterns.
*   **Implementation:**
    *   Connects to a data source containing user information (in this project, a simulated dataset is used due to data access limitations).
    *   Data points include:
        *   Previous bookings (location, property type, dates)
        *   Preferred property types (e.g., apartments, houses, unique stays)
        *   Search patterns (locations, amenities)
        *   Stay durations
        *   Budget preferences
*   **Data Structure (Example):**

    ```json
    {
        "user_id": "user123",
        "previous_bookings": [
            {"location": "London", "property_type": "apartment", "dates": "2024-07-01/2024-07-08"},
            {"location": "Rome", "property_type": "hotel", "dates": "2024-10-15/2024-10-22"}
        ],
        "preferred_property_types": ["apartment", "loft"],
        "search_patterns": ["pet-friendly", "balcony"],
        "budget": {"min": 50, "max": 150}
    }
    ```

### 3. Orchestrator Agent (Main LLM Agent)

*   **Responsibility:** Integrates data from the other agents, fetches Airbnb listings, ranks them, and generates personalized recommendations.
*   **Implementation:**
    *   Receives data from the Seasonal Demand & Pricing Agent and the User Behavior & Booking History Agent.
    *   Utilizes the Public Opensoft API to retrieve the top 20 most-reviewed Airbnb listings for a given location.
    *   Employs the Mistral-7B-Instruct-v0.1 LLM to:
        *   Rank listings based on:
            *   Current seasonal trends
            *   Historical user preferences
            *   Budget constraints
            *   Availability
        *   Recommend the top 2-3 listings.
    *   Dynamically updates recommendations as new data becomes available.
*   **LLM Prompt Example:**

    ```
    "Based on the following information, recommend the top 2 Airbnb listings in Paris for user user123:

    User Preferences:
    - Preferred property types: apartment, loft
    - Budget: \$50-\$150 per night
    - Past bookings: London (apartment), Rome (hotel)
    - Search patterns: pet-friendly, balcony

    Seasonal Demand:
    - High demand for Paris in July

    Airbnb Listings (Opensoft API Results):
    [
        { "listing_id": "1", "property_type": "apartment", "price": 120, "reviews": 500, "amenities": ["balcony", "wifi"] },
        { "listing_id": "2", "property_type": "loft", "price": 140, "reviews": 450, "amenities": ["pet-friendly", "kitchen"] },
        { "listing_id": "3", "property_type": "hotel", "price": 200, "reviews": 300, "amenities": ["pool", "gym"] }
    ]

    Recommendations:"
    ```

## Dependencies

*   Mistral-7B-Instruct-v0.1 through Hugging Face API
*   Google Trends API (pytrends)
*   Public Opensoft API (or alternative Airbnb listing API) Visit https://public.opendatasoft.com/ to find API
*   Python 3.7+
* Make sure the CSV file `mock_user.csv`

## Setup

1.  Install dependencies: `pip install pytrends`
2.  Obtain API keys for Google Trends and Opensoft (if required).
3. *Hugging Face API fectched through the source from: https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3
4. Mount it with google colab
```
from google.colab import drive
drive.mount('/content/drive')
```
5. Google Colab linked with the Hugging Face API key with as follows:
```
from google.colab import userdata  
userdata.get('secretName')
```

## Usage

1.  Run the main script: `python AI_room_reccomendations.py`
2.  The system will generate personalized Airbnb recommendations based on the configured parameters.

## Future Development

*   Integrate with real-time Airbnb API for accurate pricing and availability data.
*   Implement a user interface for easier interaction and feedback collection.
*   Incorporate more sophisticated machine learning models for improved recommendation accuracy.
*   Expand the range of data sources to include social media and other travel platforms.

# AI-Powered Travel Planner

## Description

This project implements an AI-powered travel planner that uses a multi-agent system and LLMs to generate personalized travel itineraries.

## Requirements

*   Python 3.7+
*   Libraries:
    *   pytrends
    *   openai (or your preferred LLM library)
    *   (Other libraries as needed, e.g., requests for API calls)
*   API Keys:
    *   OpenDataSoft
    *   Wettr API wttr.in

## Installation


1.  Install the required libraries: `pip install pytrends openai requests` (and any other necessary libraries)
2.  Set up your API keys:


## Usage

1.  Run the  script: on jupyter notebook `AITravel.ipynb`)
2.  The script will prompt you for user preferences or load them from a file.
3.  The AI Travel Planner will generate a personalized itinerary based on your preferences.

## Input

The system requires the following input:

*   **User Search Query:** A natural language query describing the user's desired travel experience (e.g., "beach vacations in the Caribbean").
*   **User Booking History:** A record of the user's past travel bookings.
*   **User Preferences (Optional):** Explicit preferences specified by the user (e.g., budget, travel style, desired activities).

## Output

The system generates a personalized travel itinerary, including:

*   **Recommended Destination(s)**
*   **Suggested Activities**
*   **Accommodation Options**
*   **Transportation Options**

## Configuration

*   You can configure the behavior of the AI Travel Planner by modifying the following parameters:
    *   `LLM_ENGINE`: The LLM engine to use (e.g., "text-davinci-003").
    *   `TEMPERATURE`: The temperature parameter for the LLM.
    *   `MAX_TOKENS`: The maximum number of tokens to generate for the LLM.
    *   `API_TIMEOUT`: The timeout for API calls.

## Notes

*   This is a simplified example and may require further development to handle more complex scenarios.
*   The accuracy of the recommendations depends on the quality of the data and the performance of the LLM.
*   Be sure to handle API keys securely and avoid exposing them in your code.

