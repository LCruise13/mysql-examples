
## check Duplicate
```
SELECT post_title, count(*) AS c FROM setare_posts WHERE post_type = 'post' AND substring(post_title, 1, 1)
                  NOT IN ('1','2','3','4','5','6','7','8','9') GROUP BY post_title HAVING c > 1 ORDER BY c DESC
```

## Search Replace
```
UPDATE  `MySQL_Table` SET  `MySQL_Table_Column` = REPLACE(`MySQL_Table_Column`, 'oldString', 'newString') WHERE  `MySQL_Table_Column` LIKE 'oldString%';

UPDATE `wp_postmeta` SET `meta_value` = REPLACE(`meta_value`, 'http://', 'https://') WHERE (`meta_key` = 'app-crack-link' AND `meta_value` LIKE 'http://%')

SELECT * FROM `wp_postmeta` WHERE (`meta_key` = 'app-crack-link' AND `meta_value` LIKE 'https://www.%');
```

## Only number
```
SELECT * FROM mixedvalues WHERE value REGEXP '^[0-9]+$';
```

## only two same value
```
select codemeli, count(*) as c from vote group by codemeli having c > 1 order by c desc
```

## INNER join
```
SELECT SUM(count) as `total`, wp_statistics_pages.* FROM `wp_statistics_pages` INNER JOIN `wp_statistics_visitor_relationships` ON `wp_statistics_pages`.`page_id` = `wp_statistics_visitor_relationships`.`page_id` INNER JOIN `wp_statistics_visitor`  ON `wp_statistics_visitor_relationships`.`visitor_id` = `wp_statistics_visitor`.`ID` WHERE `wp_statistics_pages`.`type` = 'post' and `wp_statistics_pages`.`id` = 104;
```
 
## IMPLODE
ints:
```
$query = "SELECT * FROM `$table` WHERE `$column` IN(".implode(',',$array).")";
```
strings:
```
$query = "SELECT * FROM `$table` WHERE `$column` IN('".implode("','",$array)."')";
```


## Sum a Column
```
SELECT SUM(count) as `page_count` FROM `wp_statistics_pages` WHERE `type` = 'post' and `id` = 104;
```

## Sum Prefessional
```
SELECT `ACCEPTOR`.`BG`, (Sum(`ACCEPTOR`.`AMOUNT`) - (SELECT SUM(AMOUNT) FROM DONOR WHERE BG = `ACCEPTOR`.`BG`)) as total FROM `ACCEPTOR` GROUP BY `ACCEPTOR`.`BG` HAVING total > 0 ORDER BY `ACCEPTOR`.`BG` ASC;
```
						     
## MySQL Join
```
SELECT ActivityText AS Activity, ActionText AS ApplicableAction
  FROM Activity 
  JOIN ActivityAction on Activity.ActivityId = ActivityAction.ActivityID
  JOIN Action on Action.ActionId = ActivityAction.ActionID
  ```
  
 ## MySQL Example
 ```
 SELECT SUM(count) as `total_count` FROM `wp_statistics_pages`
	INNER JOIN `wp_statistics_visitor_relationships`
        ON `wp_statistics_pages`.`page_id` = `wp_statistics_visitor_relationships`.`page_id`
    INNER JOIN `wp_statistics_visitor` 
        ON `wp_statistics_visitor_relationships`.`visitor_id` = `wp_statistics_visitor`.`ID`
  WHERE `wp_statistics_pages`.`type` = 'post' and `wp_statistics_pages`.`id` = 104;
```
						     
## Delete Post and Post Meta Join
```
DELETE p, pm
  FROM wp_posts p
 INNER 
  JOIN wp_postmeta pm
    ON pm.post_id = p.ID
 WHERE p.post_type = 'scheduled-action';
 ```
 
 ## Delete Term and Term Meta Join
 ```
  DELETE p, pm
  FROM wp_terms p
 INNER 
  JOIN wp_term_taxonomy pm
    ON pm.term_id = p.term_id
 WHERE p.slug LIKE '%absentus_location%';
 ```
 ## Delete All Transient
 ```
  DELETE FROM `wp_options` WHERE `option_name` LIKE ('%\_transient\_%')
 ```
 ## Delete All Woocommerce Session
 ```
  DELETE FROM `wp_options` WHERE `option_name` LIKE ('%\_transient\_%')
```

### Delete attachment and Post Meta Orphan
```
DELETE pm
FROM wp_postmeta pm
LEFT JOIN wp_posts p ON pm.post_id = p.ID
WHERE p.ID IS NULL LIMIT 5000;

DELETE pm FROM wp_postmeta pm LEFT JOIN wp_posts p ON pm.post_id = p.ID WHERE p.ID IS NULL

DELETE pm FROM wp_term_relationships pm LEFT JOIN wp_posts p ON pm.object_id = p.ID WHERE p.ID IS NULL;

DELETE pm FROM wp_ratings pm LEFT JOIN wp_posts p ON pm.rating_postid  = p.ID WHERE p.ID IS NULL;

DELETE p1
FROM wp_posts p1
LEFT JOIN wp_posts p2 ON p1.post_parent = p2.ID
WHERE p1.post_parent > 0 AND p1.post_type = 'attachment' AND p2.ID IS NULL;
```

### Get Post Meta With Number Post
```
SELECT `post_type`, COUNT(*) FROM `wp_posts` GROUP BY `post_type`;
```

#### Delete Complete Post With SQL
```
DELETE a,b,c,d
FROM wp_posts a
LEFT JOIN wp_term_relationships b ON ( a.ID = b.object_id )
LEFT JOIN wp_postmeta c ON ( a.ID = c.post_id )
LEFT JOIN wp_term_taxonomy d ON ( d.term_taxonomy_id = b.term_taxonomy_id )
LEFT JOIN wp_terms e ON ( e.term_id = d.term_id )
WHERE a.post_type IN('post');
```

#### Remove Post With Loop in PHP
```
       $list_ID = $wpdb->get_col("SELECT `ID` FROM `wp_posts` WHERE `post_type` NOT IN ('attachment','iosapp','page','post','product','shop_coupon','shop_order') LIMIT 5000");
        echo implode(",", $list_ID);
        echo '<br /><br />';

        // $number = $wpdb->query("DELETE FROM `wp_term_relationships` WHERE `object_id` IN(".implode(",", $list_ID).") LIMIT 1000");
        // sleep(3);
        // echo 'Tamom: '.$number;
        // sleep(2);
        // if($number>1) {
        // echo '<script>window.location.href = "'.home_url().'";</script>';	
        // }
        // exit;

        // $q = $wpdb->get_results("SELECT `ID` FROM `wp_posts` WHERE `post_type` NOT IN ('attachment','iosapp','page','post','product','shop_coupon','shop_order') LIMIT 1", ARRAY_A);
        // if(count($q) >0) {
        //     foreach($q as $PP) {
        //         wp_delete_post($PP['ID'], true);
        //     }
        //     sleep(1);
        //     echo '<script>window.location.href = "'.home_url().'";</script>';	
        //     exit;
        // } else {
        //     echo 'tamom';
        // }
        // exit;
```

``` Optimize Post
SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET AUTOCOMMIT = 0;
START TRANSACTION;
SET time_zone = "+00:00";

OPTIMIZE TABLE `wp_posts`;
```
