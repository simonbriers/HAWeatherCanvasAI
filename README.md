# weather_image_generator
Home Assistant weather image generator with OpenAI

# Custom Home Assistant Integration for DALL-E Image Generation

## Description
This custom integration for Home Assistant generates images using DALL-E, driven by prompts constructed from location and weather data. It combines data from Home Assistant, OpenWeatherMap, Googlemaps, and the OpenAI API to create unique, contextually relevant images.
Since the openai libary for python is the outdated 0.27.2, this integration makes direct calls to the latest Openai api endpoints, avoiding the installed python libaries. I didn't want the integration to update the libary to the current 1.3.5, as this would render your installation non standad and would require reinstalation on every update.
Be aware that there is a small cost involved fo the api calls to ChatGPT and to Dall-e. ou need an Opanai account and an active payment method to be able to use the api. The calls are made by services, and you can choose the model you want during setup. The tokens sent to ChatGPT are limited, the cost is negligable. To fetch a Dalle-3 image, that cost us at the moment € 0.04, for Dale-2 € 0.02. The difference in quality is huge however. Api calls are only made by services, up to you if you do this manually or in an automations once every ... . Every hour during night and day would be 24 calls and cost € 1 per day. Be aware, that's € 30 per month ! 1 picture per day comes down to € 1.25 per month.

## Features
- **Dynamic Image Generation**: Utilizes DALL-E to generate images based on ChatGPT prompts.
- **Location Awareness**: Integrates with Googlemaps: based on your location (LogLat) fom you Home Assistant, a reverse geochache is called to Googlemaps API. You need to get you API for this. Googlemaps wil return the name of the town or city, province, state and country. This data will be passed Dalle, who is smart enough to create something that looks like your location. If you would like in a famous street, it could use that as well, but this information is not passed at this mooment.
- **Weather Awareness**: Integrates with OpenWeatherMap for real-time weather data and uses location data from Home Assistant.
- **Configurable via Home Assistant**: Easy setup and configuration through the Home Assistant interface.

## Prerequisites
- **Home Assistant Installation**: Ensure you have Home Assistant running on your hardware.
- **API Keys Required**:
  - **OpenAI API Key**: For accessing DALL-E services (requires a subscription plan).
  - **OpenWeatherMap API Key**: Free key for weather data, obtained after registration.
  - **Google Maps API Key**: For location services. (Reverse geocaching to retrieve our adress from your HA coordinates)

## Installation
1. **Download and Install the Integration**:
   - Place the integration files in your Home Assistant's custom components directory.
2. **Install Dependencies**:
   - Ensure `openweathermap` integration is installed and configured in Home Assistant.

## Configuration
1. **Via Home Assistant UI**:
   - Navigate to the Integrations page and add this custom integration.
   - Enter the required API keys and other configuration options when prompted.

2. **Configuration Parameters**:
   - `api_key_openai`: Your OpenAI API key. Used for accessing DALL-E and GPT services.
   - `api_key_openweathermap`: Your OpenWeatherMap API key. Used for fetching weather data.
   - `api_key_googlemaps`: Your Google Maps API key. Used for location services.
   - `location_name`: A default or custom location for weather and image context.
   - `image_model_name`: Choice between 'dall-e-2' and 'dall-e-3' for image generation.
   - `gpt_model_name`: Select between 'gpt-3.5-turbo' or 'gpt-4' for text generation.
  
     The choice of the ChatGPT and Dalle models is final for your installation. If you want to switch, you need to uninstall and reinstall the integration.

## Entities and Services
This integration creates two entities and provides three services:

### Entities
1. **Camera Entity - `camera.dalle_weather_image`**:
   - This entity represents the image generated by the integration.
   - **Casting to Media Players**: The generated image can be cast to any suitable media player in your Home Assistant setup.
   - **Use in UI**: The image can be displayed in the Home Assistant UI using an image card, providing a visual representation of the current weather and location scenario.

2. **Sensor Entity - `sensor.weather2img_prompts`**:
   - Tracks the last used ChatGPT prompts for image generation.
   - **Attributes**:
     - `prompt_in`: The prompt input derived from weather and location data.
     - `prompt_out`: The final prompt sent to the OpenAI API for image generation.

### Services
The integration offers three services:

## Service: `load_testimage`
### Purpose
- Designed for testing the image handling and updating mechanisms of the integration.
### Functionality
- Loads a dummy image URL into the camera entity (`camera.dalle_weather_image`) for testing purposes.

## Service: `create_chatgpt_prompt`
### Purpose
- Creates a ChatGPT prompt combining location, time of day, season, and weather conditions for image generation.
### Functionality
- Retrieves the current day segment and season, and combines it with location name and weather conditions to create `chatgpt_in`.
- Processes `chatgpt_in` to generate `chatgpt_out` for use in DALL-E image generation.
- Dispatches `chatgpt_out` to update the `sensor.weather2img_prompts`.

## Service: `create_dalle_image`
### Purpose
- Generates an image using DALL-E based on the ChatGPT prompt.
### Functionality
- Retrieves `chatgpt_out` from `sensor.weather2img_prompts`.
- Uses this prompt to generate an image via DALL-E.
- Updates the `camera.dalle_weather_image` entity with the new image URL upon successful generation.

## Usage and Integration in UI
- **Generated Image Accessibility**:
  - The last generated image by `camera.dalle_weather_image` is saved in the Home Assistant configuration directory under `/local`. 
  - This enables easy integration of the image into different parts of the Home Assistant UI, such as in an image card, providing dynamic visual content based on the current environmental conditions.

## Contributing
- Contributions to enhance or fix issues in this integration are welcome.

## License
- Include details about the license here.

