### Sensors

## Time of Day

```
- platform: tod
  name: Night
  after: sunset
  before: sunrise

- platform: tod
  name: Quiet TIme
  after: '21:00'
  before: '06:00'
```

## Workday
```
- platform: workday
  country: US
  province: WI
  workdays: [mon, tue, wed, thu, fri]
