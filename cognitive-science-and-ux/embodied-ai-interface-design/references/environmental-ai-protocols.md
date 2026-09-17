# Environmental AI Protocols

## IoT Integration Specifications

### Supported Device Categories
1. **Lighting**: Dimmable white/color temperature; circadian rhythm programs
2. **Audio**: Ambient soundscapes; noise cancellation; alert tone modulation
3. **Climate**: Temperature, humidity, air quality
4. **Physical security**: Door locks, window sensors, safe-state enforcement
5. **Display**: Brightness, blue light filtering, refresh rate throttling
6. **Notification**: Vibration patterns, ambient light flashes, spatial audio direction

### Safety Bounds (User-Configurable)
```json
{
  "temperature": {"min": 18, "max": 26, "emergency_max": 30},
  "lighting": {"min_lux": 50, "max_lux": 500, "seizure_safe": true},
  "audio": {"max_db": 70, "alert_tone_safe_for_photosensitivity": true},
  "doors": {"auto_lock": false, "notify_trusted_contacts": true}
}
```

### API Pattern
- All environmental control runs through local MQTT broker
- AI agent publishes intent; home automation controller enforces safety bounds
- No cloud dependency for safety-critical functions
