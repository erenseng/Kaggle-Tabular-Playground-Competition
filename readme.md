## Kaggle Tabular Playground Series - Oct 2022

> I could not load the dataset because the dataset is too large ( 9.77GB )

[Click to see the dataset](https://www.kaggle.com/competitions/tabular-playground-series-oct-2022/data)

Dataset Description

The dataset consists of sequences of snapshots of the state of a Rocket League match, including position and velocity of all players and the ball, as well as extra information. The goal of the competition is to predict -- from a given snapshot in the game -- for each team, the probability that they will score within the next 10 seconds of game time.

The data was taken from professional Rocket League matches. Each event consists of a chronological series of frames recorded at 10 frames per second. All events begin with a kickoff, and most end in one team scoring a goal, but some are truncated and end with no goal scored due to circumstances which can cause gameplay strategies to shift, for example 1) nearing end of regulation (where the game continues until the ball touches the ground) or 2) becoming non-competitive, eg one team winning by 3+ goals with little time remaining.

Files:

    train_[0-9].csv: Train set split into 10 files. Rows are sorted by game_num, event_id, and event_time, and each event is entirely contained in one file.
    test.csv: Test set. Unlike the train set, the rows are scrambled.
    [train|test]_dtypes.csv: pandas dtypes for the columns in the train / test set, which can be pulled and passed to pd.read_csv() on the full set to read it with correct types since by default, pd.read_csv() will use 64-bit types which will waste memory. See below for example code.
    sample_submission.csv: A sample submission in the correct format.

Columns:

    game_num (train only): Unique identifier for the game from which the event was taken.

    event_id (train only): Unique identifier for the sequence of consecutive frames.

    event_time (train only): Time in seconds before the event ended, either by a goal being scored or simply when we decided to truncate the timeseries if a goal was not scored.

    ball_pos_[xyz]: Ball's position as a 3d vector.

    ball_vel_[xyz]: Ball's velocity as a 3d vector.

    For i in [0, 6):
        p{i}_pos_[xyz]: Player i's position as a 3d vector.
        p{i}_vel_[xyz]: Player i's velocity as a 3d vector.
        p{i}_boost: Player i's boost remaining, in [0, 100]. A player can consume boost to substantially increase their speed, and is required to fly up into the z dimension (besides driving up a wall, or the small air gained by a jump).
        All p{i} columns will be NaN if and only if the player is demolished (destroyed by an enemy player; will respawn within a few seconds).
        Players 0, 1, and 2 make up team A and players 3, 4, and 5 make up team B.
        The orientation vector of the player's car (which way the car is facing) does not necessarily match the player's velocity vector, and this dataset does not capture orientation data.

    For i in [0, 6):
        boost{i}_timer: Time in seconds until big boost orb i respawns, or 0 if it's available. Big boost orbs grant a full 100 boost to a player driving over it. The orb (x, y) locations are roughly [ (-61.4, -81.9), (61.4, -81.9), (-71.7, 0), (71.7, 0), (-61.4, 81.9), (61.4, 81.9) ] with z = 0. (Players can also gain boost from small boost pads across the map, but we do not capture those pads in this dataset).

    player_scoring_next (train only): Which player scores at the end of the current event, in [0, 6), or -1 if the event does not end in a goal.

    team_scoring_next (train only): Which team scores at the end of the current event (A or B), or NaN if the event does not end in a goal.

    team_[A|B]_scoring_within_10sec (train only): [Target columns] Value of 1 if team_scoring_next == [A|B] and time_before_event is in [-10, 0], otherwise 0.

    id (test and submission only): Unique identifier for each test row. Your submission should be a pair of team_A_scoring_within_10sec and team_B_scoring_within_10sec probability predictions for each id, where your predictions can range the real numbers from [0, 1].
