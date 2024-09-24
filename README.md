# GPS.app
Mobile App - GPS tracking

Kotloin - programming language

Android app for gps tracking

import android.Manifest                               
import android.content.pm.PackageManager                    
import android.location.Location                          
import android.os.Bundle                                
import android.widget.TextView                         
import androidx.appcompat.app.AppCompatActivity             
import androidx.core.app.ActivityCompat              
import com.google.android.gms.location.*       

// Деклариране на променливи за UI елементи и услуги за местоположение
class MainActivity : AppCompatActivity() {
 private lateinit var fusedLocationClient: FusedLocationProviderClient               
    private lateinit var locationRequest: LocationRequest                               
    private lateinit var                                                                
 locationCallback: LocationCallback

private lateinit var                                                               
 latitudeTextView: TextView
    private lateinit var longitudeTextView: TextView                                  
    private lateinit var explanationTextView: TextView

override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

// Инициализиране на UI елементи
latitudeTextView = findViewById(R.id.latitude_text)
longitudeTextView = findViewById(R.id.longitude_text)
explanationTextView = findViewById(R.id.explanation_text)
    
// Инициализиране на услуги за местоположение
fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
        locationRequest = LocationRequest.create().apply   
{ 
// Задаване на интервал и приоритет за актуализации на местоположението
            interval = 10000
            fastestInterval = 5000
            priority = LocationRequest.PRIORITY_BALANCED_POWER_ACCURACY
}
  locationCallback = object : LocationCallback() {override fun onLocationResult(locationResult:LocationResult)}
{
  super.onLocationResult(locationResult)
  val location: Location? = locationResult.lastLocation
  if (location != null)   
  { 
  // Извличане на географска ширина и дължина от обекта за местоположение
      val latitude = location.latitude
      val longitude = location.longitude
  // Актуализиране на потребителския интерфейс с получените координати
      latitudeTextView.text = "Latitude: $latitude"
      longitudeTextView.text = "Longitude: $longitude"
  // Примерен "Geocoding API" намерен в интернет
val latitude = 40.7128
val longitude = -74.0060

val geocodingUrl = "https://maps.googleapis.com/maps/api/geocode/json?" +
        "latlng=$latitude,$longitude&key=YOUR_API_KEY"
val response = URL(geocodingUrl).readText()                                                 

val jsonObject = JSONObject(response)                                                        
val results = jsonObject.getJSONArray("results")
val firstResult = results.getJSONObject(0)
val formattedAddress = firstResult.getString("formatted_address")   

explanationTextView.text = "Explanation: $formattedAddress"
               }
            }
        }
//Проверка дали са дадени разрешения за местоположение
checkPermissions()
}
private fun checkPermissions() {
    if (ActivityCompat.checkSelfPermission(this,Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
{
// Поискване на разрешения за местоположение
ActivityCompat.requestPermissions(this,arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),REQUEST_CODE_LOCATION)
        }
}
else
{ 
//Aко ли не, стартирайте актуализации на местоположението, ако са дадени разрешения
 startLocationUpdates() }
 }
private fun startLocationUpdates()
{
//Започнете да изисквате актуализации на местоположението
  fusedLocationClient.requestLocationUpdates( locationRequest,locationCallback,null)
}
// (място за още код за геокодиране и показване на обяснения)
}




    
