package ReqResRequests;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.locks.AbstractQueuedLongSynchronizer.ConditionObject;



import org.json.*;

public class Assignment {

	
	 private static final String API_KEY = "b6907d289e10d714a6e88b30761fae22";
	    private static final String API_BASE_URL = "https://samples.openweathermap.org/data/2.5/forecast/hourly?q=London,us&appid=" + API_KEY;

	    public static void main(String[] args) {
	        Assignment app = new Assignment();
	        app.run();
	    }

	    private void run() {
	        try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))) {
	            int choice;

	            do {
	                System.out.println("1. Get weather\n2. Get Wind Speed\n3. Get Pressure\n0. Exit");
	                System.out.print("Enter your choice: ");
	                choice = Integer.parseInt(reader.readLine());

	                switch (choice) {
	                    case 1:
	                        getWeatherForDate(reader);
	                        break;
	                    case 2:
	                        getWindSpeedForDate(reader);
	                        break;
	                    case 3:
	                        getPressureForDate(reader);
	                        break;
	                    case 0:
	                        System.out.println("Exiting the program.");
	                        break;
	                    default:
	                        System.out.println("Invalid choice. Please try again.");
	                }
	            } while (choice != 0);
	        } catch (IOException e) {
	            e.printStackTrace();
	        }
	    }

	    private void getWeatherForDate(BufferedReader reader) throws IOException {
	        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
	        String inputDate = reader.readLine();
	        String jsonResponse = fetchDataFromAPI();

	        // Parse JSON response and get the weather data for the input date
	        JSONArray weatherList = new JSONObject(jsonResponse).getJSONArray("list");
	        for (int i = 0; i < weatherList.length(); i++) {
	            JSONObject weatherData = weatherList.getJSONObject(i);
	            String dateTime = weatherData.getString("dt_txt");
	            if (dateTime.equals(inputDate)) {
	                double temperature = weatherData.getJSONObject("main").getDouble("temp");
	                System.out.println("Temperature for " + inputDate + ": " + temperature + "°C");
	                return;
	            }
	        }

	        System.out.println("Weather data not found for the input date.");
	    }

	    private void getWindSpeedForDate(BufferedReader reader) throws IOException {
	        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
	        String inputDate = reader.readLine();
	        String jsonResponse = fetchDataFromAPI();

	        // Parse JSON response and get the wind speed data for the input date
	        JSONArray weatherList = new JSONObject(jsonResponse).getJSONArray("list");
	        for (int i = 0; i < weatherList.length(); i++) {
	            JSONObject weatherData = weatherList.getJSONObject(i);
	            String dateTime = weatherData.getString("dt_txt");
	            if (dateTime.equals(inputDate)) {
	                double windSpeed = weatherData.getJSONObject("wind").getDouble("speed");
	                System.out.println("Wind Speed for " + inputDate + ": " + windSpeed + " m/s");
	                return;
	            }
	        }

	        System.out.println("Wind speed data not found for the input date.");
	    }

	    private void getPressureForDate(BufferedReader reader) throws IOException {
	        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
	        String inputDate = reader.readLine();
	        String jsonResponse = fetchDataFromAPI();

	        // Parse JSON response and get the pressure data for the input date
	        JSONArray weatherList = new JSONObject(jsonResponse).getJSONArray("list");
	        for (int i = 0; i < weatherList.length(); i++) {
	            JSONObject weatherData = weatherList.getJSONObject(i);
	            String dateTime = weatherData.getString("dt_txt");
	            if (dateTime.equals(inputDate)) {
	                double pressure = weatherData.getJSONObject("main").getDouble("pressure");
	                System.out.println("Pressure for " + inputDate + ": " + pressure + " hPa");
	                return;
	            }
	        }

	        System.out.println("Pressure data not found for the input date.");
	    }

	    private String fetchDataFromAPI() throws IOException {
	        URL url = new URL(API_BASE_URL);
	        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
	        connection.setRequestMethod("GET");

	        StringBuilder response = new StringBuilder();
	        try (BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
	            String line;
	            while ((line = reader.readLine()) != null) {
	                response.append(line);
	            }
	        }

	        connection.disconnect();
	        return response.toString();
	    }
	}
	






