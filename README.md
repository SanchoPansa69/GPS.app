# GPS.app
Mobile App - GPS tracking

Kotloin - programming language
Android app for gps tracking

import android.Manifest                                      - библиотека, която предоставя достъп до манифестния файл на Android, който съдържа разрешенията на приложението.

import android.content.pm.PackageManager                    - библиотеката помага да се провери дали приложението има необходимите разрешения.

import android.location.Location                           - библиотеката предоставя класове за представяне и работа с данни за местоположение.

import android.os.Bundle                                - Тази библиотека се използва за предаване на данни между дейности.

import android.widget.TextView                         - Тази библиотека се използва за показване на текст на екрана.

import androidx.appcompat.app.AppCompatActivity             - Тази библиотека предоставя основния клас за повечето дейности в приложенията за Android.

import androidx.core.app.ActivityCompat              - Тази библиотека помага за изискване на разрешения от потребителя.

import com.google.android.gms.location.*           -  Тази библиотека предоставя достъп до услуги за местоположение на устройства с Android. Той използва услугите на Google Play.


class MainActivity : AppCompatActivity() {

    private lateinit var fusedLocationClient: FusedLocationProviderClient               -> /fusedLocationClient: Използва се за достъп до услугите за местоположение на устройството./
    private lateinit var locationRequest: LocationRequest                               -> /FusedLocationProviderClient комбинира данни от различни сензори, за да предостави по-точно местоположението./
    private lateinit var                                                                -> /locationCallback: Използва се за получаване на актуализации на местоположението./
 locationCallback: LocationCallback

    private lateinit var                                                               ->  /latitudeTextView: Използва за показване на стойността на географската ширина на екрана./
 latitudeTextView: TextView
    private lateinit var longitudeTextView: TextView                                  ->   /longitudeTextView: Използва за показване на стойността на географската дължина на екрана./
    private lateinit var explanationTextView: TextView                                  -> /explanationTextView: Ще се използва за показване на по-подробно информация./

    override fun onCreate(savedInstanceState: Bundle?) {                              -> / Създаване на дейността./
        super.onCreate(savedInstanceState)                                             
        setContentView(R.layout.activity_main)                                          -> / Това казва на системата да създаде оформлението и да го покаже на екрана./


        latitudeTextView = findViewById(R.id.latitude_text)                           

        longitudeTextView = findViewById(R.id.longitude_text)
        explanationTextView = findViewById(R.id.explanation_text)                     -> / намиране и инициализиране на променлива /

        fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)          -> / Този обект ще се използва за заявяване на актуализации на местоположението. /
        locationRequest = LocationRequest.create().apply {
            interval = 10000                                                                  -> / Създава нов обект на заявка. /
            fastestInterval                                                                    -> / Определя минималния интервал от време (в милисекунди) между актуализациите на местоположението./
 = 5000
            priority = LocationRequest.PRIORITY_BALANCED_POWER_ACCURACY                         -> / Определя желаната точност и баланс на консумацията на енергия за актуализации на местоположението. /

        }

        locationCallback = object : LocationCallback() {
            override fun onLocationResult(locationResult: LocationResult) {              -> / Този метод бива извикан всеки път, когато се получи нов резултат за местоположение и гарантира изпълнението му. /
                super.onLocationResult(locationResult)   

                val location: Location? = locationResult.lastLocation                         
                if (location != null)                                                    -> / Този ред започва условен блок, който проверява дали местоположението не е нула.
 {                                                                                           Ако местоположението е налично, кодът вътре в блока ще бъде изпълнен./
                    val latitude = location.latitude
                    val longitude = location.longitude                                -> / Извличат се стойностите на гео. ширина и дължина/

                    latitudeTextView.text = "Latitude: $latitude"
                    longitudeTextView.text = "Longitude: $longitude"                 -> / Показване на стойностите/


                    / Geocoding API
val latitude = 40.7128
val longitude = -74.0060

val geocodingUrl = "https://maps.googleapis.com/maps/api/geocode/json?" +
        "latlng=$latitude,$longitude&key=YOUR_API_KEY"
val response = URL(geocodingUrl).readText()                                                 ->   / HTTP заявка към API за геокодиране./

val jsonObject = JSONObject(response)                                                        ->  / Анализиране отговора (приемайки JSON формат)./
val results = jsonObject.getJSONArray("results")
val firstResult = results.getJSONObject(0)
val formattedAddress = firstResult.getString("formatted_address")   

explanationTextView.text = "Explanation: $formattedAddress"                                 ->  / Показване на форматирания адрес./
                }
            }
        }

        checkPermissions()
    }

    private fun checkPermissions() {
        if (ActivityCompat.checkSelfPermission(                           -> / Тази функция проверява дали приложението има необходимото разрешение за достъп до точното местоположение на потребителя./
                this,
                Manifest.permission.ACCESS_FINE_LOCATION   

            ) != PackageManager.PERMISSION_GRANTED
        ) {
            ActivityCompat.requestPermissions(
                this,
                arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
                REQUEST_CODE_LOCATION
            )
        } else                                                    ->/ Ако няма ,трябва обновяване на данните./
 {
            startLocationUpdates()
        }
    }

    private fun startLocationUpdates() {                                        ->/ Тази функция започва да изисква актуализации на местоположението от FusedLocationProviderClient./
        fusedLocationClient.requestLocationUpdates(
            locationRequest,
            locationCallback,   

            null
        )
    }

    // ... (остатъка от кода за геокодиране и показване на данните)
}
