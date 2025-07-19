# Product Ingredient Agent Documentation

## 1. Introduction

The Product Ingredient Agent is a Streamlit-based application designed to analyze product ingredients using Google's Gemini AI and Tavily Search. It provides users with insights into food and personal care products by analyzing their ingredient lists, identifying potential concerns, and offering nutritional information. This documentation provides a comprehensive overview of the project, its architecture, setup, usage, and codebase.

### Key Features:

*   **Example Products**: Pre-loaded examples for quick analysis.
*   **Image Upload**: Users can upload their own product images for analysis.
*   **Camera Capture**: Direct photo capture through the application for real-time analysis.
*   **AI Analysis**: Leverages Google's Gemini 2.0 Flash for intelligent ingredient analysis.
*   **Ingredient Insights**: Provides detailed analysis of ingredients, their implications, and potential health concerns.
*   **Tavily Search Integration**: Utilizes Tavily Search for contextual information and evidence-based insights.




## 2. Architecture

The Product Ingredient Agent is built as a Streamlit application, providing an interactive web interface for users. The core functionality relies on a multimodal AI agent powered by Google Gemini and integrated with Tavily Search for enhanced information retrieval. The architecture can be broken down into the following components:

### 2.1. Frontend (Streamlit)

Streamlit is used to create the user interface, allowing users to interact with the application through a web browser. It handles:

*   **User Input**: Providing options to select example products, upload images, or capture photos via the camera.
*   **Display**: Presenting analyzed images and the AI-generated analysis in a user-friendly Markdown format.
*   **Session Management**: Maintaining the state of selected examples and analysis triggers.

### 2.2. Backend (Python with Phidata Agent)

The backend logic is primarily handled by a Python application that utilizes the `phidata` library to orchestrate the AI agent. Key elements include:

*   **Agent Initialization**: An `Agent` object from `phidata` is initialized with a Gemini model (`gemini-2.0-flash`), system prompts, and instructions. This agent is responsible for processing user requests and generating responses.
*   **Google Gemini AI**: The `Gemini` model is the core AI component, performing the multimodal analysis of product images and their ingredient lists. It interprets visual information and applies its knowledge to provide insights.
*   **Tavily Search Integration**: `TavilyTools` is integrated into the agent, allowing it to perform web searches to gather additional context, verify information, and provide evidence-based recommendations. This enhances the accuracy and depth of the ingredient analysis.
*   **Image Processing**: The application includes functions to resize images for display and to save uploaded or captured images temporarily for analysis by the AI agent.

### 2.3. Data Flow

1.  **User Interaction**: The user interacts with the Streamlit frontend by selecting an example product, uploading an image, or taking a photo.
2.  **Image Handling**: If an image is uploaded or captured, it is temporarily saved to the local file system.
3.  **Agent Invocation**: The `analyze_image` function is called, which initializes or retrieves the `phidata` agent.
4.  **AI Analysis**: The agent's `run` method is invoked with the image path. The Gemini model processes the image, extracts ingredient information, and performs analysis based on the `SYSTEM_PROMPT` and `INSTRUCTIONS`.
5.  **External Search (Optional)**: If the agent requires additional information, it uses `TavilyTools` to perform web searches.
6.  **Response Generation**: The agent generates a detailed analysis in Markdown format.
7.  **Display Results**: The Markdown response is then rendered and displayed on the Streamlit frontend.
8.  **Cleanup**: Temporary image files are deleted after analysis.

### 2.4. Configuration

API keys for Tavily and Google Gemini are loaded from Streamlit secrets, ensuring secure handling of sensitive credentials. The `constants.py` file defines the `SYSTEM_PROMPT` and `INSTRUCTIONS` that guide the AI agent's behavior and analysis methodology.




## 3. Setup and Installation

To set up and run the Product Ingredient Agent, follow these steps:

### 3.1. Prerequisites

*   Python 3.8 or higher
*   `pip` (Python package installer)
*   Git (for cloning the repository)

### 3.2. Installation Steps

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/yourusername/Product-Ingredient-Agent.git
    cd Product-Ingredient-Agent
    ```
    *(Note: Replace `yourusername` with the actual GitHub username if available, otherwise assume a placeholder.)*

2.  **Create a virtual environment**:
    It is highly recommended to use a virtual environment to manage project dependencies.
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: .\venv\Scripts\activate
    ```

3.  **Install dependencies**:
    Install all required Python packages using `pip` and the `requirements.txt` file.
    ```bash
    pip install -r requirements.txt
    ```

### 3.3. Configuration

1.  **API Keys**: The application requires API keys for Tavily and Google Gemini. Create a `.env` file in the project root directory with the following content:
    ```env
    TAVILY_API_KEY = your_tavily_api_key
    GOOGLE_API_KEY = your_gemini_api_key
    ```
    *Replace `your_tavily_api_key` and `your_gemini_api_key` with your actual API keys. These keys are accessed via `st.secrets` in the `app.py` file, which is a secure way to handle secrets in Streamlit applications.*

2.  **Example Images (Optional)**:
    If you wish to use the example products feature, place your example images in the `images/` directory. The `app.py` file references these images by their paths.
    ```
    images/
    ├── hide_and_seek.jpg
    ├── bournvita.jpg
    ├── lays.jpg
    └── shampoo.jpg
    ```

### 3.4. Running the Application

1.  **Start the Streamlit app**:
    ```bash
    streamlit run app.py
    ```

2.  **Access the application**: Open your web browser and navigate to `http://localhost:8501` (or the address provided by Streamlit in your terminal).

### 3.5. Usage

Once the application is running, you can analyze products using one of the following methods:

*   **Select from example products**: Choose from the pre-loaded examples on the 'Example Products' tab.
*   **Upload your own image**: Use the 'Upload Image' tab to upload a clear image of a product's ingredient list.
*   **Take a photo**: Utilize the 'Take Photo' tab to capture an image directly using your device's camera.

After selecting or providing an image, click the 'Analyze' button to get a detailed ingredient analysis from the AI agent.




## 4. Codebase Overview

The project codebase is structured for clarity and maintainability, with distinct files handling different aspects of the application. Below is a breakdown of the key files and their roles:

### 4.1. `app.py`

This is the main Streamlit application file. It orchestrates the user interface, handles user interactions, and integrates with the AI agent for ingredient analysis. Key functionalities include:

*   **Streamlit UI**: Sets up the page configuration, title, and tabs for different input methods (example products, image upload, camera capture).
*   **Image Handling**: Contains `resize_image_for_display` to optimize images for UI display and `save_uploaded_file` to temporarily store user-provided images.
*   **Agent Initialization**: The `get_agent()` function (cached for performance) initializes the `phidata` Agent with the Gemini model and integrates Tavily Search tools.
*   **Analysis Logic**: The `analyze_image()` function triggers the AI analysis by calling the agent's `run` method with the image path and displays the Markdown-formatted response.
*   **Session State Management**: Utilizes `st.session_state` to manage the selected example product and analysis trigger, ensuring a smooth user experience across interactions.
*   **Environment Variables**: Loads API keys from Streamlit secrets (`st.secrets`), which are populated from the `.env` file.

### 4.2. `constants.py`

This file stores global constants used throughout the application, primarily defining the behavior and instructions for the AI agent:

*   **`SYSTEM_PROMPT`**: A detailed prompt that defines the AI agent's persona as an 


expert Food Product Analyst, specializing in ingredient analysis and nutrition science. This prompt guides the agent to provide health insights, identify potential concerns, and utilize nutritional knowledge and research to offer evidence-based insights.
*   **`INSTRUCTIONS`**: A set of specific instructions for the AI agent on how to perform the ingredient analysis. These include reading ingredient lists, simplifying explanations, identifying artificial additives, checking dietary restrictions, rating nutritional value, highlighting health implications, suggesting alternatives, and providing evidence-based recommendations.

### 4.3. `requirements.txt`

This file lists all the Python dependencies required to run the application. It ensures that all necessary libraries are installed in the correct versions, facilitating easy setup and deployment. Key dependencies include:

*   `streamlit`: For building the web application.
*   `phidata`: The framework for building and managing the AI agent.
*   `pillow`: For image processing functionalities.
*   `tavily-python`: For integrating with the Tavily Search API.
*   `google-generativeai`: For interacting with Google Gemini AI models.

### 4.4. `images/`

This directory contains example product images that are pre-loaded into the application for demonstration purposes. These images are referenced in `app.py` and provide a convenient way for users to test the application without needing to upload their own images immediately.




## 5. Future Enhancements

This project provides a solid foundation for ingredient analysis using AI. Several enhancements can be considered to further improve its functionality and user experience:

*   **Expanded AI Capabilities**: Integrate more advanced AI models or fine-tune the existing Gemini model for even more nuanced ingredient analysis, including deeper understanding of chemical interactions, allergen identification, and personalized dietary recommendations.
*   **Database Integration**: Implement a database to store product information, ingredient analyses, and user preferences. This would allow for faster retrieval of previously analyzed products and enable features like user profiles and historical analysis.
*   **Multi-language Support**: Extend the application to support ingredient analysis in multiple languages, catering to a broader global audience.
*   **Mobile Responsiveness**: Optimize the Streamlit interface for better responsiveness on mobile devices, enhancing the experience for users capturing photos on the go.
*   **Advanced Search Filters**: Allow users to filter or search for products based on specific ingredients, dietary restrictions, or nutritional values.
*   **Community Features**: Implement features that allow users to share their analyses, contribute to a community-driven ingredient database, or rate products.
*   **Integration with Product Databases**: Connect with external product databases (e.g., Open Food Facts, USDA FoodData Central) to enrich product information and provide more comprehensive insights.
*   **Real-time Ingredient Scanning**: Explore possibilities for real-time ingredient scanning using device cameras, providing instant feedback as users point their camera at product labels.
*   **User Authentication**: Implement user authentication to personalize the experience, save user data, and provide premium features.
*   **Reporting and Export**: Allow users to generate and export detailed reports of ingredient analyses in various formats (e.g., PDF, CSV).


