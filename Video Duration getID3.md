# Getting Video Duration using getID3

## 📦 1. Install getID3 via Composer

To get started, install the **getID3** library using Composer:

```bash
composer require james-heinrich/getid3
```

## 🧠 2. In your theme’s .theme file (e.g., example.theme), add the following logic inside example_preprocess_node():

```bash
use Drupal\file\Entity\File;

function example_preprocess_node(&$variables) {
  $language = \Drupal::languageManager()->getCurrentLanguage();
  $variables['language'] = $language;
  $node = $variables['node'];

  if ($node->bundle() == 'media' || $node->bundle() == 'event') {
    $video_duration = '00:00';

    if ($node->hasField('field_video') && !$node->get('field_video')->isEmpty()) {
      $media = $node->get('field_video')->entity;

      if ($media && $media->hasField('field_media_video_file') && !$media->get('field_media_video_file')->isEmpty()) {
        $file = $media->get('field_media_video_file')->entity;

        if ($file instanceof File) {
          $file_uri = $file->getFileUri();
          $file_path = \Drupal::service('file_system')->realpath($file_uri);

          if (file_exists($file_path)) {
            $getID3 = new getID3();
            $videoInfo = $getID3->analyze($file_path);

            $duration = !empty($videoInfo['playtime_seconds']) ? (int) $videoInfo['playtime_seconds'] : 0;
            $video_duration = $duration >= 3600 ? gmdate('H:i:s', $duration) : gmdate('i:s', $duration);
          }
        }
      }
    }

    $variables['video_duration'] = $video_duration;
  }
}
```

## 🖼️ 3. Display in Twig

Now that the video duration is passed to the template, you can render it in your node Twig template like this:
```bash
<p>Video Duration: {{ video_duration }}</p>
```
