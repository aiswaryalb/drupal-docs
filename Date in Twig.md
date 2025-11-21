

## Date normal cases 
```
{{ node.getCreatedTime|date('U')|format_date('custom', 'j M Y') }}
```
```
{{ node.field_date_media.value|date('U')|format_date('custom', 'j M Y') }}
```

## Block-content-type
```
{% if content.field_start_time[0]['#markup'] is defined and content.field_end_time[0]['#markup'] is defined %}
  {{content.field_start_time ? content.field_start_time[0]['#markup']|date('U')|format_date('custom', 'g:i A') : ''}} -
  {{content.field_end_time ? content.field_end_time[0]['#markup']|date('U')|format_date('custom', 'g:i A') : ''}}
{% endif %}
```

## Date range - time
	{{ node.field_date.start_date|date('g:i A') }} -{{ node.field_date.end_date|date('g:i A') }}

## Date and time - time
  ```
  {{ paragraph.field_date.date|date('g:i A') }}
  ```
