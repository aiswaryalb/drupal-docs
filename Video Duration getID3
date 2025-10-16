📼 How to Get Video Duration in Drupal Using getID3

In this note, we’ll cover how to use the getID3 PHP library in Drupal to extract the duration of a video file and make it accessible in a Twig template.

📦 1. Install getID3 via Composer
composer require james-heinrich/getid3

🧠 2. Add Logic to example.theme

In your theme’s .theme file (e.g., example.theme), add the following logic inside example_preprocess_node():

use getID3;
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

🖼️ 3. Display in Twig

Now that the video duration is passed to the template, you can render it in your node Twig template like this:

<p>Video Duration: {{ video_duration }}</p>
