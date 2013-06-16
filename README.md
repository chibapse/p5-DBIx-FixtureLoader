# NAME

DBIx::FixtureLoader - Loading fixtures and inserting to your database

# SYNOPSIS

    use DBI;
    use DBIx::FixtureLoader;
    

    my $dbh = DBI->connect(...);
    my $loader = DBIx::FixtureLoader->new(dbh => $dbh);
    $loader->load_fixture('item.csv');

# DESCRIPTION

DBIx::FixtureLoader is to load fixture data and insert to your database.

# INTEFACE

## Constructor

    $loader = DBIx::FixtureLoader->new(%option)

`new` is Constructor method. Various options may be set in `%option`, which affect
the behaviour of the object (Type and defaults in parentheses):

### `dbh (DBI::db)`

Required. Database handler.

### `bulk_insert (Bool)`

Using bulk\_insert or not. Default value is depend on your database.

### `update (Bool, Default: false)`

Using `INSERT ON DUPLICATE` or not. It can be used only works on `mysql`.

### `csv_option (HashRef, Default: +{})`

Specifying [Text::CSV](http://search.cpan.org/perldoc?Text::CSV)'s option. `binary` and `blank_is_undef`
are automatically set.

## Methods

### `$loader->load_fixture($file_or_data:(Str|HashRef|ArrayRef), [%option])`

Loading fixture and inserting to your database. Table name and file format is guessed from
file name. For example, "item.csv" contains data of "item" table and format is "CSV".

In most cases `%option` is not needed. Available keys of `%option` are as follows.

- `table:Str`

    table name of database.

- `format:Str`

    data format. "CSV", "YAML" and "JSON" are available.

- `update:Bool`

    Using `ON DUPLICATE KEY UPDATE` or not. Default value depends on object setting.

## File Name and Data Format

### file name

Data format is guessed from extension. Table name is guessed from basename. Leading alphabets,
underscores and numbers are considered table name. So, `"user_item-2.csv"` is considered CSV format
and containing data of "user\_item" table.

### data format

"CSV", "YAML" and "JSON" are parsable. CSV file must have header line for determining column names.

Datas in "YAML" or "JSON" must be ArrayRef or HashRef containing HashRefs. Each HashRef is the data
of database record and keys of HashRef is matching to column names of the table.

# LICENSE

Copyright (C) Masayuki Matsuki.

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# AUTHOR

Masayuki Matsuki <y.songmu@gmail.com>
