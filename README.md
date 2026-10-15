<div align="center">

# QASRA Salah API

### Quran and Sunnah Research Academy, Ajmer

**Prayer intervals · Past & future Hijri date lookup · Multilingual calendar information**

Free to use in your websites and applications with academy attribution.

[Try the API](https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata) · [Features](#what-you-can-build-with-qasra) · [Date lookup](#past-present-and-future-date-lookup) · [Quick start](#quick-start) · [Response reference](#response-reference)

</div>

---

QASRA Salah API is a service by **Quran and Sunnah Research Academy, Ajmer**. One request returns prayer intervals for both Hanafi and Shafi, alongside Gregorian and estimated Hijri date information for your selected location and date.

| Prayer times | Calendar information | Integration |
| --- | --- | --- |
| Hanafi and Shafi in one response | Gregorian date and estimated Hijri date | Simple HTTP GET requests |
| Start and end times for each interval | Hijri month names in English, Urdu and Arabic | JSON responses |
| Times formatted in your chosen timezone | Weekday names in English and Arabic | Browser access with CORS |

## What you can build with QASRA

QASRA brings prayer intervals and calendar information together in a single response. Use it to create a daily prayer dashboard, add an Islamic date widget to a website, show both madhhabs in a mobile application, or let visitors explore dates from the past and future.

| Feature | What your users get |
| --- | --- |
| Location-based prayer information | Prayer intervals for the supplied latitude, longitude and date. No city-name lookup is needed. |
| Two madhhabs together | Hanafi and Shafi results from one request, ready for a madhhab selector or side-by-side display. |
| Start and end times | Both boundaries of each interval, so your interface can show a prayer's time window. |
| Sunrise reference | A separate sunrise entry alongside the five prayer entries. |
| Overnight Isha interval | Isha's end is calculated using the following morning's Fajr boundary. |
| Today's date automatically | Omit `date` to use today in the selected timezone. |
| Historical date lookup | Supply a past Gregorian date to retrieve its estimated Hijri day, month, year and weekday. |
| Future date lookup | Supply a future Gregorian date to preview its estimated Hijri date and retrieve prayer intervals. |
| Weekday lookup | Find the weekday for a supplied Gregorian date in English and Arabic. |
| Three Hijri month languages | English, Urdu and Arabic names plus a numeric month value for your interface. |
| Timezone selection | Format prayer times in a supplied IANA timezone. Choose the timezone appropriate to the location. |
| Manual Hijri day adjustment | An optional `offset` for a day-number adjustment when needed. |
| Browser-ready integration | JSON, CORS support and no API key requirement. |
| Free website and app usage | Integrate with clear attribution to Quran and Sunnah Research Academy, Ajmer. |

## Past, present and future date lookup

**“What was the Hijri date on 10 April 1994?”** and **“What could the Hijri date be on 10 April 2030?”** are both questions you can explore with this API. Set the Gregorian `date` and read the returned Hijri day, month and year. The same response includes the weekday and prayer intervals for your selected location.

You do not need separate requests for the Hijri date, weekday and prayer times: all are returned together.

### Look up a past date

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=1994-04-10'
```

For this request, the current implementation returns:

| Information | Value |
| --- | --- |
| Gregorian date | 10 April 1994 |
| Estimated Hijri date | 28 Shawwal 1414 |
| Hijri month in Urdu | شوال |
| Hijri month in Arabic | شوال |
| Weekday | Sunday / الأحد |

Use this feature for date exploration, personal date records, anniversary pages and calendar interfaces. A calculated historical Hijri date is an estimate, not a verified record of a historical moon-sighting announcement.

### Preview a future date

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=2030-04-10'
```

For this request, the current implementation returns:

| Information | Value |
| --- | --- |
| Gregorian date | 10 April 2030 |
| Estimated Hijri date | 6 Dhul-Hijjah 1451 |
| Hijri month in Urdu | ذو الحجہ |
| Hijri month in Arabic | ذو الحجة |
| Weekday | Wednesday / الأربعاء |

This is useful for calendar previews and advance planning. Future Hijri dates remain estimates and can differ from subsequently announced local dates.

### Get today's information

Omit `date` to retrieve today's information in your chosen timezone:

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata'
```

A website can show today's prayer intervals, Gregorian date, estimated Hijri date and weekday from this one response.

### Date lookup scope

Use valid Gregorian dates in `YYYY-MM-DD` format. These examples demonstrate past and future lookups; an unrestricted range covering every historical era or every possible year has not been verified. Dates are interpreted using the Gregorian calendar, and Hijri output is an algorithmic estimate. The API accepts a Gregorian date as input; reverse lookup from a Hijri date is not provided.

## Find the weekday for a date

**Find which day of the week a past date fell on, or which weekday a future date will fall on.** Supply a valid Gregorian date using the `date` parameter; the response includes its weekday in both English and Arabic, alongside prayer times and estimated Hijri date information.

For example, to look up **15 August 1947**:

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=1947-08-15'
```

Read these fields in the response:

```json
{
  "en": "Friday",
  "ar": "الجمعة"
}
```

They are available at `date.hijri.weekday.en` and `date.hijri.weekday.ar`. Despite their location in the response, they describe the **requested Gregorian date**. Coordinates are still required for the API request.

This makes the API useful for historical date lookups, anniversary features and future calendar planning. Use valid dates in `YYYY-MM-DD` format. Historical dates are interpreted using the Gregorian calendar; converting dates from other historical calendar systems is outside this feature. Weekday lookup is separate from the estimated Hijri date.

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

## Free to use & attribution

You may use QASRA Salah API **free of charge** in your websites and applications. When displaying information from this API, you must clearly credit **Quran and Sunnah Research Academy, Ajmer** as the provider.

Suggested attribution for your website or application:

> Prayer times and Hijri date information provided by Quran and Sunnah Research Academy, Ajmer (QASRA).

You can link the credit to [QASRA Salah API](https://salah.qasra.org/).

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

See the [complete example response](examples/response.json), generated from the local Worker for Ajmer on `2026-10-15` with `Asia/Kolkata` and no Hijri offset. It includes every response field and both sets of prayer intervals.

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

## Display calendar information in your application

Using the `data` returned by the JavaScript example above:

```javascript
const hijri = data.date.hijri;

const englishDate = `${hijri.day} ${hijri.month.en} ${hijri.year} AH`;
const urduMonth = hijri.month.ur;
const arabicMonth = hijri.month.ar;
const weekday = `${hijri.weekday.en} / ${hijri.weekday.ar}`;

console.log(data.date.gregorian);
console.log(englishDate);
console.log(urduMonth, arabicMonth);
console.log(weekday);
```

`day` and `year` are strings; `month.number` is numeric. Urdu and Arabic month names are returned as text, while the day and year use Western digits. Apply your own numeral formatting and right-to-left presentation where your interface needs it.

## Python example

This example uses Python's standard library:

```python
import json
from urllib.parse import urlencode
from urllib.request import urlopen

params = urlencode({
    "latitude": 26.4499,
    "longitude": 74.6399,
    "timezone": "Asia/Kolkata",
    "date": "1994-04-10",
})

with urlopen("https://salah.qasra.org/?" + params, timeout=15) as response:
    data = json.load(response)

hijri = data["date"]["hijri"]
print(hijri["day"], hijri["month"]["en"], hijri["year"])
print(hijri["weekday"]["en"])
print(data["timings"]["hanafi"]["Fajr"])
```

## Build a calendar or prayer timetable

For a month view, request each Gregorian date you want to display, using the same location and timezone. Save each response under its Gregorian date, then use the returned calendar fields and prayer intervals to build your rows or day cards.

| Interface | Response fields to use |
| --- | --- |
| Daily prayer dashboard | `timings.hanafi` or `timings.shafi` |
| Madhhab selector | Switch between the two `timings` objects locally. |
| Hijri date widget | `date.hijri.day`, `date.hijri.month`, `date.hijri.year` |
| Historical date explorer | Explicit `date` parameter, then `date.hijri` |
| Future calendar preview | Explicit future `date`, then calendar fields and prayer intervals |
| Multilingual calendar heading | `month.en`, `month.ur`, `month.ar`, `weekday.en`, `weekday.ar` |
| Monthly prayer timetable | One request per Gregorian day, with cached results |

The API supplies the data; your application handles layouts, reminders, notifications and calendar navigation. It returns formatted time strings rather than timestamps, so date-aware countdowns should account for the timezone and Isha's next-day end.

## Frequently asked questions

**Can I use it on my website or mobile app for free?**

Yes. Clearly credit Quran and Sunnah Research Academy, Ajmer as the provider. See [attribution](#free-to-use--attribution).

**Can I find the estimated Hijri date for a birthday or another past date?**

Yes. Supply that Gregorian date along with coordinates. The response includes estimated Hijri day, month, year and the Gregorian date's weekday.

**Can I request a future date?**

Yes. A valid future Gregorian date can be supplied in the same way. Treat its Hijri date as an estimate.

**Do I need to choose Hanafi or Shafi in the request?**

No. Both are returned together. Select the appropriate object in your application.

**Does changing the timezone change the supplied coordinates?**

No. Coordinates specify the location; timezone controls time formatting and the selection of today when `date` is omitted.

**Does `offset` change prayer timings?**

No. It adjusts only the returned Hijri day number. See its month-boundary behavior in [calendar information](#calendar-information).

**Does it return moon-sighting announcements, Islamic events or holidays?**

No. It provides estimated Hijri date information. Event lists, holiday announcements and observational records are not returned.

**Can I use a city name instead of coordinates?**

The endpoint requires latitude and longitude. Convert a city name into coordinates in your application before requesting data.

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
