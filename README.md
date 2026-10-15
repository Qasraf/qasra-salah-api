<div align="center">

# QASRA Salah API

### Quran and Sunnah Research Academy, Ajmer

**Prayer times and Hijri date information for your apps and websites.**

[Try the API](https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata) · [Quick start](#quick-start) · [Response reference](#response-reference)

</div>

---

QASRA Salah API is a service by **Quran and Sunnah Research Academy, Ajmer**. One request returns prayer intervals for both Hanafi and Shafi, alongside Gregorian and estimated Hijri date information for your selected location and date.

| Prayer times | Calendar information | Integration |
| --- | --- | --- |
| Hanafi and Shafi in one response | Gregorian date and estimated Hijri date | Simple HTTP GET requests |
| Start and end times for each interval | Hijri month names in English, Urdu and Arabic | JSON responses |
| Times formatted in your chosen timezone | Weekday names in English and Arabic | Browser access with CORS |

## Quick start

**Base URL:** `https://salah.qasra.org/`

Provide latitude and longitude to get today's results. The default timezone is `Asia/Kolkata`.

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata'
```

To request a particular date:

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=2026-10-15'
```

No API key is required by this endpoint.

## Request parameters

| Parameter | Required | Default | Description |
| --- | --- | --- | --- |
| `latitude` | Yes | — | Location latitude in decimal degrees, between -90 and 90. |
| `longitude` | Yes | — | Location longitude in decimal degrees, between -180 and 180. |
| `timezone` | No | `Asia/Kolkata` | IANA timezone name, such as `Asia/Kolkata`, `Asia/Dubai` or `Europe/London`. |
| `date` | No | Today in the selected timezone | Gregorian date in `YYYY-MM-DD` format. |
| `offset` | No | `0` | Integer adjustment to the returned Hijri day number. Does not change prayer times. See [calendar information](#calendar-information). |

Use valid coordinates, dates and timezone names. The coordinate ranges above describe valid input; callers should validate their input before sending a request.

## Response reference

Successful requests return a JSON object with these fields:

| Field | Description |
| --- | --- |
| `method` | Academy name: `Quran and Sunnah Research Academy, Ajmer`. |
| `note` | Information about the prayer intervals. |
| `coordinates` | Requested numeric `latitude` and `longitude`. |
| `timezone` | Timezone used to format prayer times. |
| `date.gregorian` | Requested Gregorian date, or today's date when omitted. |
| `date.hijri` | Estimated Hijri day, month, year and weekday information. |
| `timings.hanafi` | Hanafi prayer intervals. |
| `timings.shafi` | Shafi prayer intervals. |

### Prayer intervals

Both `timings.hanafi` and `timings.shafi` contain the following entries. Each entry has a `start` and an `end`, formatted as a 12-hour time string such as `5:15 AM`.

| Entry | Description |
| --- | --- |
| `Fajr` | Fajr interval. |
| `Sunrise` | Sunrise-to-midday reference interval. |
| `Dhuhr` | Dhuhr interval. |
| `Asr` | Asr interval for the corresponding madhhab. |
| `Maghrib` | Maghrib interval. |
| `Isha` | Isha interval, ending the following morning. |

Read each interval's own `start` and `end`: the end of one interval may differ from the start of the next. `Sunrise` is a reference entry, not an additional obligatory prayer. Isha crosses midnight; its end belongs to the next day. Time strings do not include a date or UTC offset.

### Calendar information

Each response includes calendar information for **one requested day**:

```json
{
  "gregorian": "2026-10-15",
  "hijri": {
    "day": "3",
    "month": {
      "number": 5,
      "en": "Jamadi al-Awwal",
      "ur": "جمادی الاولیٰ",
      "ar": "جمادى الأولى"
    },
    "year": "1448",
    "weekday": {
      "en": "Thursday",
      "ar": "الخميس"
    }
  }
}
```

This is an illustrative `date` object from the current implementation. Hijri dates are estimates and do not constitute a local moon-sighting announcement.

The optional `offset` adds to the Hijri day number only. It does not normalize the Hijri month or year at month boundaries; use it only when the resulting day is appropriate. Weekday fields describe the requested Gregorian date and do not shift with `offset`.

To build a monthly calendar, request each Gregorian date individually. The endpoint does not return a whole month or an Islamic events calendar in one request.

## JavaScript example

```javascript
async function getQasraSalahTimes({ latitude, longitude, timezone = 'Asia/Kolkata', date }) {
  const url = new URL('https://salah.qasra.org/');
  url.searchParams.set('latitude', String(latitude));
  url.searchParams.set('longitude', String(longitude));
  url.searchParams.set('timezone', timezone);
  if (date) url.searchParams.set('date', date);

  const response = await fetch(url);
  const data = await response.json();
  if (!response.ok) throw new Error(data.error || 'Unable to fetch prayer times');
  return data;
}

const data = await getQasraSalahTimes({
  latitude: 26.4499,
  longitude: 74.6399,
  timezone: 'Asia/Kolkata',
  date: '2026-10-15'
});

console.log(data.timings.hanafi.Fajr.start);
console.log(data.timings.shafi.Asr.start);
console.log(data.date.hijri.month.ur);
```

## HTTP behavior

| Status | Meaning |
| --- | --- |
| `200` | Response generated. Validate inputs and returned values before displaying them. |
| `400` | Latitude or longitude is missing or empty. |
| `500` | A calculation or formatting error occurred, for example an invalid timezone. |

Error responses contain an `error` string:

```json
{
  "error": "Please provide 'latitude' and 'longitude' parameters."
}
```

Invalid dates or malformed coordinates are not consistently rejected by the current endpoint and may produce invalid values in a `200` response. Integrations should validate input rather than rely on the HTTP status alone.

The API supports browser CORS requests and `OPTIONS` preflight. Successful responses carry a one-day cache policy (`86400` seconds). When requesting today's information, refresh the date at local midnight and include an explicit `date` in the URL so that each day's request has its own cache key.

---

<div align="center">

**QASRA Salah API**  
A service by **Quran and Sunnah Research Academy, Ajmer**

</div>
