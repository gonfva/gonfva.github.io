---
title: "Mantis on SQL Server"
date: 2010-03-09T11:45:46
categories:
  - Developer
  - got ya
toc: true
tags:
  - Developer
---

[Technical post]

I've been busy last couple of days trying to make [Mantis](http://www.mantisbt.org/) work with a Microsoft SQL Server database on a Windows Server 2003. Many people would ask why to use MSSQL instead of Mysql, a much better DBMS, not mention the OS election. I'm not planning to enter in a flame war. I wanted a free bug tracking system that worked with MSSQL. Candidates were Bugzilla, with [higly experimental MSSQL support](http://www.zplace.com/), [BugTracker .NET](http://ifdefined.com/bugtrackernet.html), much settled on the platform but looking a bit amateur, and Mantis, the final election, which promised MSSQL support.

The fact is that it hasn't been a trivial task for me. I'm a Java (former VB.NET) developer, and I've had to fight a lot. Just in case someone else has the same problems, here I write what I've done [edited] to install **Mantis 1.2.0**:

- **Install PHP**. Some tricks here. _First_. Use PHP 5.2. PHP 5.3 needs other configuration. _Second_. I installed in IIS as an ISAPI filter. _Third_. Install MSSQL extension in the PHP installation. _Fourth_. You need [ntwdblib.dll](http://www.dlldll.com/ntwdblib.dll_download.html) in `...\system32 folder`.

- **Unzip Mantis 1.2.0 and install as a new virtual project.**

- **Link PHP with IIS**: No tricks here. I followed [this post in Spanish](http://www.desarrolloweb.com/articulos/instalando-php-con-iis.html).

- **Create de Mantis SQL schema**: The installation procedure of Mantis creates the original database (of an ancient version of Mantis) and in the same script upgrades the tables. I don't know the reason why it doesn't work in MSSQL but the fact is [it doesn't work](http://www.mantisbt.org/forums/viewtopic.php?f=3&t=9920). So you have to create the SQL schema on hand. Here is my script:

```sql
CREATE TABLE mantis_config_table (
    config_id VARCHAR(64) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    access_reqd INT DEFAULT 0,
    type INT DEFAULT 90,
    value TEXT NOT NULL,
    PRIMARY KEY (config_id, project_id, user_id)
);

CREATE INDEX idx_config ON mantis_config_table (config_id);

CREATE TABLE mantis_bug_file_table (
    id INT IDENTITY(1,1) NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    title VARCHAR(250) DEFAULT '' NOT NULL,
    description VARCHAR(250) DEFAULT '' NOT NULL,
    diskfile VARCHAR(250) DEFAULT '' NOT NULL,
    filename VARCHAR(250) DEFAULT '' NOT NULL,
    folder VARCHAR(250) DEFAULT '' NOT NULL,
    filesize INT DEFAULT 0 NOT NULL,
    file_type VARCHAR(250) DEFAULT '' NOT NULL,
    date_added INT DEFAULT 1 NOT NULL,
    content IMAGE NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_bug_file_bug_id ON mantis_bug_file_table (bug_id);

ALTER TABLE mantis_bug_file_table ADD
    user_id INT DEFAULT 0 NOT NULL;

CREATE TABLE mantis_bug_history_table (
    id INT IDENTITY(1,1) NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    date_modified INT DEFAULT 1 NOT NULL,
    field_name VARCHAR(32) DEFAULT '' NOT NULL,
    old_value VARCHAR(128) DEFAULT '' NOT NULL,
    new_value VARCHAR(128) DEFAULT '' NOT NULL,
    type SMALLINT DEFAULT 0 NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_bug_history_bug_id ON mantis_bug_history_table (bug_id);

CREATE INDEX idx_history_user_id ON mantis_bug_history_table (user_id);

CREATE TABLE mantis_bug_monitor_table (
    user_id INT DEFAULT 0 NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    PRIMARY KEY (user_id, bug_id)
);

CREATE TABLE mantis_bug_relationship_table (
    id INT IDENTITY(1,1) NOT NULL,
    source_bug_id INT DEFAULT 0 NOT NULL,
    destination_bug_id INT DEFAULT 0 NOT NULL,
    relationship_type SMALLINT DEFAULT 0 NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_relationship_source ON mantis_bug_relationship_table (source_bug_id);

CREATE INDEX idx_relationship_destination ON mantis_bug_relationship_table (destination_bug_id);

CREATE TABLE mantis_bug_table (
    id INT IDENTITY(1,1) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    reporter_id INT DEFAULT 0 NOT NULL,
    handler_id INT DEFAULT 0 NOT NULL,
    duplicate_id INT DEFAULT 0 NOT NULL,
    priority SMALLINT DEFAULT 30 NOT NULL,
    severity SMALLINT DEFAULT 50 NOT NULL,
    reproducibility SMALLINT DEFAULT 10 NOT NULL,
    status SMALLINT DEFAULT 10 NOT NULL,
    resolution SMALLINT DEFAULT 10 NOT NULL,
    projection SMALLINT DEFAULT 10 NOT NULL,
    eta SMALLINT DEFAULT 10 NOT NULL,
    bug_text_id INT DEFAULT 0 NOT NULL,
    os VARCHAR(32) DEFAULT '' NOT NULL,
    os_build VARCHAR(32) DEFAULT '' NOT NULL,
    platform VARCHAR(32) DEFAULT '' NOT NULL,
    version VARCHAR(64) DEFAULT '' NOT NULL,
    fixed_in_version VARCHAR(64) DEFAULT '' NOT NULL,
    build VARCHAR(32) DEFAULT '' NOT NULL,
    profile_id INT DEFAULT 0 NOT NULL,
    view_state SMALLINT DEFAULT 10 NOT NULL,
    summary VARCHAR(128) DEFAULT '' NOT NULL,
    sponsorship_total INT DEFAULT 0 NOT NULL,
    sticky BIT DEFAULT '0' NOT NULL,
    due_date INT DEFAULT 1 NOT NULL,
    date_submitted INT DEFAULT 1 NOT NULL,
    last_updated INT DEFAULT 1 NOT NULL,
    category_id INT DEFAULT 1 NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_bug_sponsorship_total ON mantis_bug_table (sponsorship_total);

CREATE INDEX idx_bug_fixed_in_version ON mantis_bug_table (fixed_in_version);

CREATE INDEX idx_bug_status ON mantis_bug_table (status);

CREATE INDEX idx_project ON mantis_bug_table (project_id);

CREATE TABLE mantis_bug_text_table (
    id INT IDENTITY(1,1) NOT NULL,
    description TEXT NOT NULL,
    steps_to_reproduce TEXT NOT NULL,
    additional_information TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_bugnote_table (
    id INT IDENTITY(1,1) NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    reporter_id INT DEFAULT 0 NOT NULL,
    bugnote_text_id INT DEFAULT 0 NOT NULL,
    view_state SMALLINT DEFAULT 10 NOT NULL,
    date_submitted INT DEFAULT 1 NOT NULL,
    last_modified INT DEFAULT 1 NOT NULL,
    note_type INT DEFAULT 0,
    note_attr VARCHAR(250) DEFAULT '',
    PRIMARY KEY (id)
);

CREATE INDEX idx_bug ON mantis_bugnote_table (bug_id);

CREATE INDEX idx_last_mod ON mantis_bugnote_table (last_modified);

CREATE TABLE mantis_bugnote_text_table (
    id INT IDENTITY(1,1) NOT NULL,
    note TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_custom_field_project_table (
    field_id INT DEFAULT 0 NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    sequence SMALLINT DEFAULT 0 NOT NULL,
    PRIMARY KEY (field_id, project_id)
);

CREATE TABLE mantis_custom_field_string_table (
    field_id INT DEFAULT 0 NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    value VARCHAR(255) DEFAULT '' NOT NULL,
    PRIMARY KEY (field_id, bug_id)
);

CREATE INDEX idx_custom_field_bug ON mantis_custom_field_string_table (bug_id);

CREATE TABLE mantis_custom_field_table (
    id INT IDENTITY(1,1) NOT NULL,
    name VARCHAR(64) DEFAULT '' NOT NULL,
    type SMALLINT DEFAULT 0 NOT NULL,
    possible_values TEXT DEFAULT '' NOT NULL,
    default_value VARCHAR(255) DEFAULT '' NOT NULL,
    valid_regexp VARCHAR(255) DEFAULT '' NOT NULL,
    access_level_r SMALLINT DEFAULT 0 NOT NULL,
    access_level_rw SMALLINT DEFAULT 0 NOT NULL,
    length_min INT DEFAULT 0 NOT NULL,
    length_max INT DEFAULT 0 NOT NULL,
    require_report BIT DEFAULT '0' NOT NULL,
    require_update BIT DEFAULT '0' NOT NULL,
    display_report BIT DEFAULT '0' NOT NULL,
    display_update BIT DEFAULT '1' NOT NULL,
    require_resolved BIT DEFAULT '0' NOT NULL,
    display_resolved BIT DEFAULT '0' NOT NULL,
    display_closed BIT DEFAULT '0' NOT NULL,
    require_closed BIT DEFAULT '0' NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_custom_field_name ON mantis_custom_field_table (name);

ALTER TABLE mantis_custom_field_table ADD
    filter_by BIT DEFAULT '1' NOT NULL;

CREATE TABLE mantis_filters_table (
    id INT IDENTITY(1,1) NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    is_public BIT DEFAULT NULL,
    name VARCHAR(64) DEFAULT '' NOT NULL,
    filter_string TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_news_table (
    id INT IDENTITY(1,1) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    poster_id INT DEFAULT 0 NOT NULL,
    date_posted INT DEFAULT 1 NOT NULL,
    last_modified INT DEFAULT 1 NOT NULL,
    view_state SMALLINT DEFAULT 10 NOT NULL,
    announcement BIT DEFAULT '0' NOT NULL,
    headline VARCHAR(64) DEFAULT '' NOT NULL,
    body TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_project_category_table (
    project_id INT DEFAULT 0 NOT NULL,
    category VARCHAR(64) DEFAULT '' NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    PRIMARY KEY (project_id, category)
);

CREATE TABLE mantis_project_file_table (
    id INT IDENTITY(1,1) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    title VARCHAR(250) DEFAULT '' NOT NULL,
    description VARCHAR(250) DEFAULT '' NOT NULL,
    diskfile VARCHAR(250) DEFAULT '' NOT NULL,
    filename VARCHAR(250) DEFAULT '' NOT NULL,
    folder VARCHAR(250) DEFAULT '' NOT NULL,
    filesize INT DEFAULT 0 NOT NULL,
    file_type VARCHAR(250) DEFAULT '' NOT NULL,
    date_added INT DEFAULT 1 NOT NULL,
    content IMAGE NOT NULL,
    PRIMARY KEY (id)
);

ALTER TABLE mantis_project_file_table ADD
    user_id INT DEFAULT 0 NOT NULL;

CREATE TABLE mantis_project_hierarchy_table (
    child_id INT NOT NULL,
    parent_id INT NOT NULL
);

CREATE TABLE mantis_project_table (
    id INT IDENTITY(1,1) NOT NULL,
    name VARCHAR(128) DEFAULT '' NOT NULL,
    status SMALLINT DEFAULT 10 NOT NULL,
    enabled BIT DEFAULT '1' NOT NULL,
    view_state SMALLINT DEFAULT 10 NOT NULL,
    access_min SMALLINT DEFAULT 10 NOT NULL,
    file_path VARCHAR(250) DEFAULT '' NOT NULL,
    description TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_project_id ON mantis_project_table (id);

CREATE UNIQUE INDEX idx_project_name ON mantis_project_table (name);

CREATE INDEX idx_project_view ON mantis_project_table (view_state);

CREATE TABLE mantis_project_user_list_table (
    project_id INT DEFAULT 0 NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    access_level SMALLINT DEFAULT 10 NOT NULL,
    PRIMARY KEY (project_id, user_id)
);

CREATE INDEX idx_project_user ON mantis_project_user_list_table (user_id);

CREATE TABLE mantis_project_version_table (
    id INT IDENTITY(1,1) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    version VARCHAR(64) DEFAULT '' NOT NULL,
    date_order INT DEFAULT 1 NOT NULL,
    description TEXT NOT NULL,
    released BIT DEFAULT '1' NOT NULL,
    PRIMARY KEY (id)
);

CREATE UNIQUE INDEX idx_project_version ON mantis_project_version_table (project_id, version);

CREATE TABLE mantis_sponsorship_table (
    id INT IDENTITY(1,1) NOT NULL,
    bug_id INT DEFAULT 0 NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    amount INT DEFAULT 0 NOT NULL,
    logo VARCHAR(128) DEFAULT '' NOT NULL,
    url VARCHAR(128) DEFAULT '' NOT NULL,
    paid BIT DEFAULT '0' NOT NULL,
    date_submitted INT DEFAULT 1 NOT NULL,
    last_updated INT DEFAULT 1 NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_sponsorship_bug_id ON mantis_sponsorship_table (bug_id);

CREATE INDEX idx_sponsorship_user_id ON mantis_sponsorship_table (user_id);

CREATE TABLE mantis_tokens_table (
    id INT IDENTITY(1,1) NOT NULL,
    owner INT NOT NULL,
    type INT NOT NULL,
    timestamp INT DEFAULT 1 NOT NULL,
    expiry INT DEFAULT 1 NOT NULL,
    value TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_user_pref_table (
    id INT IDENTITY(1,1) NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    default_profile INT DEFAULT 0 NOT NULL,
    default_project INT DEFAULT 0 NOT NULL,
    refresh_delay INT DEFAULT 0 NOT NULL,
    redirect_delay INT DEFAULT 0 NOT NULL,
    bugnote_order VARCHAR(4) DEFAULT 'ASC' NOT NULL,
    email_on_new BIT DEFAULT '0' NOT NULL,
    email_on_assigned BIT DEFAULT '0' NOT NULL,
    email_on_feedback BIT DEFAULT '0' NOT NULL,
    email_on_resolved BIT DEFAULT '0' NOT NULL,
    email_on_closed BIT DEFAULT '0' NOT NULL,
    email_on_reopened BIT DEFAULT '0' NOT NULL,
    email_on_bugnote BIT DEFAULT '0' NOT NULL,
    email_on_status BIT DEFAULT '0' NOT NULL,
    email_on_priority BIT DEFAULT '0' NOT NULL,
    email_on_priority_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_status_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_bugnote_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_reopened_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_closed_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_resolved_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_feedback_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_assigned_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_on_new_min_severity SMALLINT DEFAULT 10 NOT NULL,
    email_bugnote_limit SMALLINT DEFAULT 0 NOT NULL,
    language VARCHAR(32) DEFAULT 'english' NOT NULL,
    PRIMARY KEY (id)
);

ALTER TABLE mantis_user_pref_table ADD
    timezone VARCHAR(32) DEFAULT '' NOT NULL;

CREATE TABLE mantis_user_print_pref_table (
    user_id INT DEFAULT 0 NOT NULL,
    print_pref VARCHAR(27) DEFAULT '' NOT NULL,
    PRIMARY KEY (user_id)
);

CREATE TABLE mantis_user_profile_table (
    id INT IDENTITY(1,1) NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    platform VARCHAR(32) DEFAULT '' NOT NULL,
    os VARCHAR(32) DEFAULT '' NOT NULL,
    os_build VARCHAR(32) DEFAULT '' NOT NULL,
    description TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE mantis_user_table (
    id INT IDENTITY(1,1) NOT NULL,
    username VARCHAR(32) DEFAULT '' NOT NULL,
    realname VARCHAR(64) DEFAULT '' NOT NULL,
    email VARCHAR(64) DEFAULT '' NOT NULL,
    password VARCHAR(32) DEFAULT '' NOT NULL,
    date_created INT DEFAULT 1 NOT NULL,
    last_visit INT DEFAULT 1 NOT NULL,
    enabled BIT DEFAULT '1' NOT NULL,
    protected BIT DEFAULT '0' NOT NULL,
    access_level SMALLINT DEFAULT 10 NOT NULL,
    login_count INT DEFAULT 0 NOT NULL,
    lost_password_request_count SMALLINT DEFAULT 0 NOT NULL,
    failed_login_count SMALLINT DEFAULT 0 NOT NULL,
    cookie_string VARCHAR(64) DEFAULT '' NOT NULL,
    PRIMARY KEY (id)
);

CREATE UNIQUE INDEX idx_user_cookie_string ON mantis_user_table (cookie_string);

CREATE UNIQUE INDEX idx_user_username ON mantis_user_table (username);

CREATE INDEX idx_enable ON mantis_user_table (enabled);

CREATE INDEX idx_access ON mantis_user_table (access_level);

INSERT INTO mantis_user_table(username, realname, email, password, date_created, last_visit, enabled, protected, access_level, login_count, lost_password_request_count, failed_login_count, cookie_string) VALUES
('administrator', '', 'root@localhost', '63a9f0ea7bb98050796b649e85481845', 1, 1, '1', '0', 90, 3, 0, 0, '43c90a58b766eca71a4eaec08120f1b51fec152d8f4d2c0cfec26390ed94f575');

ALTER TABLE mantis_bug_history_table ALTER COLUMN old_value VARCHAR(255) NOT NULL;

ALTER TABLE mantis_bug_history_table ALTER COLUMN new_value VARCHAR(255) NOT NULL;

CREATE TABLE mantis_email_table (
    email_id INT IDENTITY(1,1) NOT NULL,
    email VARCHAR(64) DEFAULT '' NOT NULL,
    subject VARCHAR(250) DEFAULT '' NOT NULL,
    submitted INT DEFAULT 1 NOT NULL,
    metadata TEXT NOT NULL,
    body TEXT NOT NULL,
    PRIMARY KEY (email_id)
);

CREATE INDEX idx_email_id ON mantis_email_table (email_id);

ALTER TABLE mantis_bug_table ADD
    target_version VARCHAR(64) DEFAULT '' NOT NULL;

ALTER TABLE mantis_bugnote_table ADD
    time_tracking INT DEFAULT 0 NOT NULL;

CREATE INDEX idx_diskfile ON mantis_bug_file_table (diskfile);

ALTER TABLE mantis_user_print_pref_table ALTER COLUMN print_pref VARCHAR(64) NOT NULL;

ALTER TABLE mantis_bug_history_table ALTER COLUMN field_name VARCHAR(64) NOT NULL;

CREATE TABLE mantis_tag_table (
    id INT IDENTITY(1,1) NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    name VARCHAR(100) DEFAULT '' NOT NULL,
    description TEXT NOT NULL,
    date_created INT DEFAULT 1 NOT NULL,
    date_updated INT DEFAULT 1 NOT NULL,
    PRIMARY KEY (id, name)
);

CREATE TABLE mantis_bug_tag_table (
    bug_id INT DEFAULT 0 NOT NULL,
    tag_id INT DEFAULT 0 NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    date_attached INT DEFAULT 1 NOT NULL,
    PRIMARY KEY (bug_id, tag_id)
);

CREATE INDEX idx_typeowner ON mantis_tokens_table (type, owner);

CREATE TABLE mantis_plugin_table (
    basename VARCHAR(40) NOT NULL,
    enabled BIT DEFAULT '0' NOT NULL,
    PRIMARY KEY (basename)
);

CREATE TABLE mantis_category_table (
    id INT IDENTITY(1,1) NOT NULL,
    project_id INT DEFAULT 0 NOT NULL,
    user_id INT DEFAULT 0 NOT NULL,
    name VARCHAR(128) DEFAULT '' NOT NULL,
    status INT DEFAULT 0 NOT NULL,
    PRIMARY KEY (id)
);

CREATE UNIQUE INDEX idx_category_project_name ON mantis_category_table (project_id, name);

INSERT INTO mantis_category_table
( project_id, user_id, name, status ) VALUES
( '0', '0', 'General', '0' ) ;

DROP TABLE mantis_project_category_table;

ALTER TABLE mantis_project_table ADD
    category_id INT DEFAULT 1 NOT NULL;

INSERT INTO mantis_plugin_table
    ( basename, enabled ) VALUES
    ( 'MantisCoreFormatting', '1' );

ALTER TABLE mantis_project_table ADD
    inherit_global INT DEFAULT 0 NOT NULL;

ALTER TABLE mantis_project_hierarchy_table ADD
    inherit_parent INT DEFAULT 0 NOT NULL;

ALTER TABLE mantis_plugin_table ADD
    protected BIT DEFAULT '0' NOT NULL,
    priority INT DEFAULT 3 NOT NULL;

ALTER TABLE mantis_project_version_table ADD
    obsolete BIT DEFAULT '0' NOT NULL;

CREATE TABLE mantis_bug_revision_table (
    id INT IDENTITY(1,1) NOT NULL,
    bug_id INT NOT NULL,
    bugnote_id INT DEFAULT 0 NOT NULL,
    user_id INT NOT NULL,
    timestamp INT DEFAULT 1 NOT NULL,
    type INT NOT NULL,
    value TEXT NOT NULL,
    PRIMARY KEY (id)
);

CREATE INDEX idx_bug_rev_id_time ON mantis_bug_revision_table (bug_id, timestamp);

CREATE INDEX idx_bug_rev_type ON mantis_bug_revision_table (type);

CREATE INDEX idx_project_hierarchy_child_id ON mantis_project_hierarchy_table (child_id);

CREATE INDEX idx_project_hierarchy_parent_id ON mantis_project_hierarchy_table (parent_id);

CREATE INDEX idx_tag_name ON mantis_tag_table (name);

CREATE INDEX idx_bug_tag_tag_id ON mantis_bug_tag_table (tag_id);

INSERT INTO mantis_config_table ( value, type, access_reqd, config_id, project_id, user_id ) VALUES ('182', 1, 90, 'database_version', 0, 0 );
```

- **Create a config_inc.php file:** No tricks here. You have to enter database connection data. Mine is more complicated but I guess something like this could do the trick:

```php
<?php
$g_hostname = '***';
$g_db_type = 'mssql';
$g_database_name = '***';
$g_db_username = '**_';
$g_db_password = '_**';
?>
```

- **The final trick:** Don't ask me why (and don't ask me how I discovered), but the fact is that you have to edit the file ...\library\adodb\adodb.inc.php, and change the sentence `define('ADODB_FETCH_ASSOC',2);` to `define('ADODB_FETCH_ASSOC',0);`
