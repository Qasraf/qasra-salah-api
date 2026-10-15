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
| Times formatted in your chosen timezone | Weekday names in English, Urdu and Arabic | Browser access with CORS |

> **Calendar & timetable features:** Calendar-only lookup, monthly calendars and prayer timetables, date ranges, both date formats, Hijri offset rollover and Urdu weekdays. See the [endpoint guide](#calendar-and-timetable-endpoints).

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
| Weekday lookup | Find the weekday for a supplied Gregorian date in English, Urdu and Arabic. |
| Three Hijri month languages | English, Urdu and Arabic names plus a numeric month value for your interface. |
| Timezone selection | Format prayer times in a supplied IANA timezone. Choose the timezone appropriate to the location. |
| Manual Hijri day adjustment | An optional `offset` for a Hijri date adjustment when needed. |
| Browser-ready integration | JSON, CORS support and no API key requirement. |
| Free website and app usage | Integrate with clear attribution to Quran and Sunnah Research Academy, Ajmer. |

## Past, present and future date lookup

**“What was the Hijri date on 10 April 1994?”** and **“What could the Hijri date be on 10 April 2030?”** are both questions you can explore with this API. Set the Gregorian `date` and read the returned Hijri day, month and year. The same response includes the weekday and prayer intervals for your selected location.

You do not need separate requests for the Hijri date, weekday and prayer times: all are returned together.

### Look up a past date

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=1994-04-10'
```

This request returns:

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

This request returns:

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

Supply a valid Gregorian date in `YYYY-MM-DD` or `DD/MM/YYYY` format, within the supported year range `0623`–`9999`. Hijri dates are algorithmic estimates. The API accepts a Gregorian date as input; reverse lookup from a Hijri date is not provided.

## Find the weekday for a date

**Find which day of the week a past date fell on, or which weekday a future date will fall on.** Supply a valid Gregorian date using the `date` parameter; the response includes its weekday in English, Urdu and Arabic, alongside prayer times and estimated Hijri date information.

For example, to look up **15 August 1947**:

```bash
curl 'https://salah.qasra.org/?latitude=26.4499&longitude=74.6399&timezone=Asia/Kolkata&date=1947-08-15'
```

Read these fields in the response:

```json
{
  "en": "Friday",
  "ar": "الجمعة",
  "ur": "جمعہ"
}
```

They are available at `date.hijri.weekday.en`, `date.hijri.weekday.ur` and `date.hijri.weekday.ar`. Despite their location in the response, they describe the **requested Gregorian date**. For a weekday or Hijri-date lookup without coordinates, use `/calendar?date=1947-08-15`. Prayer requests require coordinates.

This makes the API useful for historical date lookups, anniversary features and future calendar planning. Use valid dates in `YYYY-MM-DD` or `DD/MM/YYYY` format. Historical dates are interpreted using the Gregorian calendar; converting dates from other historical calendar systems is outside this feature. Weekday lookup is separate from the estimated Hijri date.

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

No API key is required. For calendar information without coordinates, try [`/calendar?date=10/04/1994`](https://salah.qasra.org/calendar?date=10%2F04%2F1994).

## Free to use & attribution

You may use QASRA Salah API **free of charge** in your websites and applications. When displaying information from this API, you must clearly credit **Quran and Sunnah Research Academy, Ajmer** as the provider.

Suggested attribution for your website or application:

> Prayer times and Hijri date information provided by Quran and Sunnah Research Academy, Ajmer (QASRA).

You can link the credit to [QASRA Salah API](https://salah.qasra.org/).

## Request parameters

| Parameter | Required | Default | Description |
| --- | --- | --- | --- |
| `latitude` | Prayer requests | — | Location latitude in decimal degrees, between -90 and 90. |
| `longitude` | Prayer requests | — | Location longitude in decimal degrees, between -180 and 180. |
| `timezone` | No | `Asia/Kolkata` | IANA timezone name, such as `Asia/Kolkata`, `Asia/Dubai` or `Europe/London`. |
| `date` | No | Today in the selected timezone | Gregorian date in `YYYY-MM-DD` or `DD/MM/YYYY` format. |
| `offset` | No | `0` | Integer Hijri date adjustment from `-366` to `366`. Does not change prayer times. See [calendar information](#calendar-information). |

Use valid coordinates, dates and timezone names. The coordinate ranges above describe valid input; callers should validate their input before sending a request.

## Response reference

See the [complete response example](examples/response.json) for Ajmer on `2026-10-15` with `Asia/Kolkata` and no Hijri offset. It includes every response field and both sets of prayer intervals.

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

A single-day response includes calendar information in this shape:

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
      "ar": "الخميس",
      "ur": "جمعرات"
    }
  }
}
```

This example shows the `date` object. Hijri dates are estimates and do not constitute a local moon-sighting announcement.

`offset` shifts the conversion input before estimating the Hijri date, so month and year boundaries are handled. Weekday fields describe the requested Gregorian date and do not shift with `offset`.

Use `/calendar?year=2026&month=10` for a whole Gregorian month, or supply `from` and `to` for an inclusive date range. See the [endpoint guide](#calendar-and-timetable-endpoints). Islamic event lists are not returned.

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

For a month view, use `/calendar?year=2026&month=10`. For prayer intervals as well, use `/timetable` with `year`, `month`, coordinates and timezone. Read the ordered `data` array to build day cards or timetable rows. You can also request an inclusive custom range using `from` and `to`.

| Interface | Response fields to use |
| --- | --- |
| Daily prayer dashboard | `timings.hanafi` or `timings.shafi` |
| Madhhab selector | Switch between the two `timings` objects locally. |
| Hijri date widget | `date.hijri.day`, `date.hijri.month`, `date.hijri.year` |
| Historical date explorer | Explicit `date` parameter, then `date.hijri` |
| Future calendar preview | Explicit future `date`, then calendar fields and prayer intervals |
| Multilingual calendar heading | `month.en`, `month.ur`, `month.ar`, `weekday.en`, `weekday.ur`, `weekday.ar` |
| Monthly prayer timetable | One monthly `/timetable` request with coordinates |

The API supplies the data; your application handles layouts, reminders, notifications and calendar navigation. It returns formatted time strings rather than timestamps, so date-aware countdowns should account for the timezone and Isha's next-day end.

## Frequently asked questions

**Can I use it on my website or mobile app for free?**

Yes. Clearly credit Quran and Sunnah Research Academy, Ajmer as the provider. See [attribution](#free-to-use--attribution).

**Can I find the estimated Hijri date for a birthday or another past date?**

Yes. Supply that Gregorian date to `/calendar`; coordinates are unnecessary unless you also want prayer times. The response includes estimated Hijri day, month, year and the Gregorian date's weekday.

**Can I request a future date?**

Yes. A valid future Gregorian date can be supplied in the same way. Treat its Hijri date as an estimate.

**Do I need to choose Hanafi or Shafi in the request?**

No. Both are returned together. Select the appropriate object in your application.

**Does changing the timezone change the supplied coordinates?**

No. Coordinates specify the location; timezone controls time formatting and the selection of today when `date` is omitted.

**Does `offset` change prayer timings?**

No. It adjusts the estimated Hijri date only. Month and year boundaries are handled; see the endpoint guide below.

**Does it return moon-sighting announcements, Islamic events or holidays?**

No. It provides estimated Hijri date information. Event lists, holiday announcements and observational records are not returned.

**Can I use a city name instead of coordinates?**

Prayer requests require latitude and longitude. Convert a city name into coordinates in your application before requesting prayer data. Calendar-only requests do not need a location.

## Rate limits

Every public IP has the following allowance across all Salah API endpoints:

| Window | Requests per IP |
| --- | ---: |
| Minute | **60** |
| Hour | **300** |
| Day | **500** |

All three limits apply together. Daily lookups, calendar requests, monthly calendars and prayer timetables share this Salah allowance. A monthly timetable counts as one request, not one request for each returned day. `OPTIONS` preflight is exempt. Requests accepted by the limiter count even if the input is invalid or a result is served from the API's cache.

Minute and hour quotas use fixed windows. The daily allowance resets at **00:00 Asia/Kolkata (India time)**. Devices sharing a public IP share the allowance. Salah and QASRA Geocoder have separate allowances: using one does not consume the other's quota.

When any allowance is exhausted, the API returns **HTTP 429**. Read the `Retry-After` response header for how many seconds to wait; it is not always 60 seconds. An hourly or daily allowance can require a longer wait. Do not immediately repeat a blocked request.

A rate-limit error contains `error`, `exceeded`, `retry_after_seconds` and `timezone`. `exceeded` identifies the exhausted window(s), such as `minute`, `hour` or `day`. If the limiting service is unavailable, the API returns **503** with a retry delay instead of bypassing protection.

Fetch a timetable once and reuse it in your application rather than polling every second. Refresh when its date range or location changes. For a higher allowance, email [support@qasra.org](mailto:support@qasra.org); an email does not automatically change the limit.

## HTTP behavior

| Status | Meaning |
| --- | --- |
| `200` | Response generated. Validate inputs and returned values before displaying them. |
| `400` | Invalid or missing input: coordinates, date, timezone, offset or date selectors. |
| `429` | The shared per-IP rate limit was exceeded. |
| `503` | The rate limiting service is unavailable. |
| `422` | Prayer times are unavailable for the requested location/date. |
| `500` | An unexpected calculation error occurred. |

Error responses contain an `error` string:

```json
{
  "error": "Please provide 'latitude' and 'longitude' parameters."
}
```

Requests are validated before calculation; invalid input returns `400` with an explanatory `error` string.

The API supports browser CORS requests and `OPTIONS` preflight. Successful responses carry a one-day cache policy (`86400` seconds). When requesting today's information, refresh the date at local midnight and include an explicit `date` in the URL so that each day's request has its own cache key.

## Calendar and timetable endpoints

Base URL: `https://salah.qasra.org`. Daily, monthly and ranged responses share the same estimated Hijri calendar fields.

| Endpoint | Response | Coordinates |
| --- | --- | --- |
| `/` | Hanafi and Shafi prayer intervals plus date information | Required |
| `/calendar` | Gregorian date, estimated Hijri date and weekday; no prayer timings | Not required |
| `/timetable` | Hanafi and Shafi prayer intervals plus date information | Required |

All three endpoints accept one of these date selectors:

| Selector | Example | Result |
| --- | --- | --- |
| `date` | `date=1994-04-10` or `date=10/04/1994` | One day |
| `year` and `month` | `year=2028&month=2` | All days in that Gregorian month |
| `from` and `to` | `from=01/04/2026&to=15/04/2026` | Inclusive date range |
| No selector | — | Today in the selected timezone |

Do not mix selectors. Both ends of a range are required, as are both year and month for a month request. Date ranges are limited to 366 days. Input dates must be real Gregorian dates in exactly `YYYY-MM-DD` or `DD/MM/YYYY` form; output `date.gregorian` is always normalized to `YYYY-MM-DD`.

The accepted Gregorian year range is `0623`–`9999`. Hijri dates are algorithmic estimates and may differ from local moon-sighting dates.

### Request examples

```text
/calendar?date=10/04/1994
/calendar?date=10/04/2030
/calendar?year=2028&month=2
/calendar?from=01/04/2026&to=15/04/2026
/timetable?latitude=26.4499&longitude=74.6399&year=2026&month=10&timezone=Asia/Kolkata
/timetable?latitude=26.4499&longitude=74.6399&from=2026-12-31&to=2027-01-02
```

Use `https://salah.qasra.org` as the base URL for these paths. `/` also accepts month and range selectors.

### Response shapes

A single-day prayer response contains academy information, coordinates, timezone, calendar information and prayer intervals for both madhhabs. A single-day calendar response contains `method`, `timezone` and `date`:

```json
{
  "method": "Quran and Sunnah Research Academy, Ajmer",
  "timezone": "Asia/Kolkata",
  "date": {
    "gregorian": "1994-04-10",
    "hijri": {
      "day": "28",
      "month": { "number": 10, "en": "Shawwal", "ur": "شوال", "ar": "شوال" },
      "year": "1414",
      "weekday": { "en": "Sunday", "ar": "الأحد", "ur": "اتوار" }
    }
  }
}
```

Month and range requests return an envelope with `method`, `timezone`, `from`, `to`, `count` and `data`. Calendar `data` entries have the shape `{ "date": { ... } }`. Prayer `data` entries are complete daily prayer responses, and the envelope also contains `coordinates`. Dates are ordered from earliest to latest; each appears exactly once. Even a one-day `from`/`to` request returns this batch envelope.

### Hijri offset and Urdu weekdays

`offset` is an integer from `-366` to `366`, default `0`. It shifts the date used for Hijri estimation by that many days, with month and year rollover. Hijri dates are estimates.

`offset` affects only the estimated Hijri date; the Gregorian date, weekday and prayer timings are independent of this adjustment. Adjustments that move outside the supported Gregorian years return `400`.

`date.hijri.weekday` contains weekday names in English (`en`), Urdu (`ur`) and Arabic (`ar`):

| English | Urdu |
| --- | --- |
| Sunday | اتوار |
| Monday | پیر |
| Tuesday | منگل |
| Wednesday | بدھ |
| Thursday | جمعرات |
| Friday | جمعہ |
| Saturday | ہفتہ |

### Validation and HTTP behavior

- Latitude and longitude must be complete decimal numbers in `[-90, 90]` and `[-180, 180]`, respectively, when prayer times are requested.
- Invalid coordinates, impossible dates, invalid timezones, malformed offsets and invalid date selectors return `400` with an `error` string.
- Valid coordinates where prayer times cannot be calculated (for example polar conditions) return `422` with an explanatory error. A timetable containing an unavailable day fails as a whole; no partial response is returned.
- Unexpected calculation failures return `500`.
- The API supports CORS, `OPTIONS` preflight, 12-hour time strings and a one-day cache policy for successful responses. Error responses are not given that cache policy.
- Send an explicit date or month/range selector for predictable date-specific caching. Refresh date selectors at local midnight for daily widgets.

---

<div align="center">

**QASRA Salah API**  
A service by **Quran and Sunnah Research Academy, Ajmer**

</div>
