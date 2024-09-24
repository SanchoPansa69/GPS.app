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

// fusedLocationClient: Използва се за достъп до услугите за местоположение на устройството.
// FusedLocationProviderClient комбинира данни от различни сензори, за да предостави по-точно местоположението./
class MainActivity : AppCompatActivity() {
  private lateinit var fusedLocationClient: FusedLocationProviderClient
  private lateinit var locationRequest: LocationRequest      
  private lateinit var   
