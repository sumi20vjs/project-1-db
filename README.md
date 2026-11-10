community project database
CREATE TABLE tanker_requests (
  request_id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  quantity_litres INT NOT NULL,
  requested_date DATE NOT NULL,
  preferred_time TIME,
  priority VARCHAR(20) DEFAULT 'NORMAL',
  status VARCHAR(30) DEFAULT 'PENDING',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

SELECT * FROM tanker_requests;
INSERT INTO tanker_requests
(
    user_id,
    quantity_litres,
    requested_date,
    preferred_time,
    priority,
    status
)
VALUES
(
    2,
    5000,
    '2026-10-08',
    '10:00:00',
    'HIGH',
    'PENDING'
);

SELECT * FROM tanker_requests;
CREATE TABLE schedules (
    schedule_id INT AUTO_INCREMENT PRIMARY KEY,
    request_id INT NOT NULL,
    tanker_id INT NOT NULL,
    scheduled_date DATE NOT NULL,
    start_time TIME,
    end_time TIME,
    status VARCHAR(30) DEFAULT 'SCHEDULED',

    FOREIGN KEY (request_id)
        REFERENCES tanker_requests(request_id)
);
INSERT INTO schedules
(
    request_id,
    tanker_id,
    scheduled_date,
    start_time,
    end_time,
    status
)
VALUES
(
    1,
    1,
    '2026-10-08',
    '10:00:00',
    '11:00:00',
    'SCHEDULED'
);

SELECT * FROM schedules;

SELECT
    tr.request_id,
    tr.user_id,
    tr.quantity_litres,
    tr.requested_date,
    tr.preferred_time,
    tr.priority,
    tr.status AS request_status,
    s.schedule_id,
    s.tanker_id,
    s.scheduled_date,
    s.start_time,
    s.end_time,
    s.status AS schedule_status
FROM tanker_requests tr
LEFT JOIN schedules s
    ON tr.request_id = s.request_id;

    SELECT
    request_id,
    user_id,
    quantity_litres,
    requested_date,
    preferred_time,
    priority,
    status
FROM tanker_requests
WHERE status = 'PENDING'
ORDER BY
    priority DESC,
    requested_date ASC;

    UPDATE tanker_requests
SET status = 'APPROVED'
WHERE request_id = 1;

SELECT * FROM tanker_requests;
SELECT
    s.schedule_id,
    s.request_id,
    s.tanker_id,
    s.scheduled_date,
    s.start_time,
    s.end_time,
    s.status
FROM schedules s
WHERE s.status = 'SCHEDULED';
SELECT
    tr.request_id,
    tr.user_id,
    tr.quantity_litres,
    tr.requested_date,
    tr.preferred_time,
    tr.priority,
    tr.status AS request_status,

    s.schedule_id,
    s.tanker_id,
    s.scheduled_date,
    s.start_time,
    s.end_time,
    s.status AS schedule_status

FROM tanker_requests tr

LEFT JOIN schedules s
    ON tr.request_id = s.request_id

ORDER BY tr.request_id DESC;
