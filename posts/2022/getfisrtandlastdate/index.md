# 快速取得某天當月第一天與最後一天


```CSharp
public static DateTime FirstDayOfMonth_AddMethod(this DateTime value)
    {
        return value.Date.AddDays(1 - value.Day);
    }


public static DateTime LastDayOfMonth_NewMethodWithReuseOfExtMethod(this DateTime value)
    {
        return new DateTime(value.Year, value.Month, value.DaysInMonth());
    }

```

