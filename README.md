# Containerize This: PHP/Apache/MySQL

# Note By Qaiser

UPDATE mysql.user SET host = '%' WHERE host = 'internalfoo' AND user = 'root';
UPDATE mysql.db SET host = '%' WHERE host = 'internalfoo' AND user = 'root';
FLUSH PRIVILEGES;

### Intro

PHP VERSION `PHP 7.4.33`
