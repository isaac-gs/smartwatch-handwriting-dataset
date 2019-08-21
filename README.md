# Wearables-Driven Freeform Handwriting Dataset
The database (using PostgreSQL) is comprised of three tables and five experimental scenarios. This document will describe the dataset, database, and how to use it.

## The Dataset
Our dataset includes the sensor data for the following experimental scenarios (please refer to the paper for further details):
- Baseline Experiments
    - **E1**: The scenario in which users copied the same content.
    - **E2**: Users copied unique content from one another and across sessions.
    - **E3**: Emulating a freewriting scenario in which users experience a higher cognitive load, this experiment had users answer essay-style questions.
- Attack Experiments
    - **E4**: Attackers are given copies of their target's handwriting to practice against.
    - **E5**: Each attacker viewed the video footage of their target's wrist and hand during the writing process.

## The Database
This section describes the different tables included in our database and what the possible values mean.

### Baseline Experiments Table

    TABLE baseline_experiments (
        user_session_id TEXT PRIMARY KEY,
        user_id BIGINT NOT NULL,
        session BIGINT NOT NULL,
        scenario BIGINT NOT NULL
    )

Definitions:
- **user_session_id**: user_id + session + scenario.
- **user_id**: the anonymized ID of the user.
- **session**: 1 or 0, depending on if this was the training session.
- **scenario**: 1 (E1), 2 (E2), or 3 (E3) depending on which scenario the data is from.

### Impostor Experiments Table

    TABLE impostor_experiments (
        user_session_id TEXT PRIMARY KEY,
        target_user BIGINT NOT NULL,
        attacking_user BIGINT NOT NULL,
        experiment BIGINT NOT NULL
        attack BIGINT NOT NULL
    )

Definitions:
- **user_session_id**: attacking_user + target_user + experiment + attack
- **target_user**: the anonymized ID of the user.
- **attacking_user**: the anonymized ID of the attacker.
- **experiment**: 1 (E1), 2 (E2), or 3 (E3) depending on which scenario the training data is from.
- **attack**: 4 (E4) or 5 (E5) depending on which attack method was used for this particular set of data.

### Handwriting Data Table
        
    TABLE handwriting_data (
        timestamp TEXT NOT NULL,
        x DOUBLE PRECISION NOT NULL,
        y DOUBLE PRECISION NOT NULL,
        z DOUBLE PRECISION NOT NULL,
        user_session_id TEXT NOT NULL,
        index BIGINT NOT NULL,
        PRIMARY KEY(user_session_id, index)
    )

Definitions:
- **timestamp**: The datetime difference between the start and end of the session.
- **x**: the x component of the linear acceleration sensor data.
- **y**: the y component of the linear acceleration sensor data.
- **z**: the z component of the linear acceleration sensor data.
- **user_session_id**: user_session_id from either the baseline or impostor experiment table.
- **index**: the index of the data in a given csv file.

## Usage Instructions
The dataset was exported with pgAdmin and can be imported with the psql command.

`psql -U your_user_name your_db_name < dataset.sql`
