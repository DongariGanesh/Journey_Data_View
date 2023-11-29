# Journey_Data_View

SELECT
jou.JourneyName,
jou.VersionNumber,
j.EmailName,
jou.JourneyStatus,
COUNT(s.JobID) AS Sent,
COUNT(o.JobID) AS Opened,
COUNT(c.JobID) AS Clicked,
COUNT(b.JobID) AS Bounced,
COUNT(u.JobID) AS Unsubscribed
FROM _Job j LEFT JOIN _Sent s ON j.JobID = s.JobID
LEFT JOIN _Open o ON s.JobID = o.JobID and s.ListID = o.ListID and s.BatchID = o.BatchID and s.SubscriberID = o.SubscriberID and o.IsUnique = 1
LEFT JOIN _Click c ON s.JobID = c.JobID  and s.ListID = c.ListID and s.BatchID = c.BatchID and s.SubscriberID = c.SubscriberID and c.IsUnique = 1
LEFT JOIN _Bounce b ON s.JobID = b.JobID and s.ListID = b.ListID and s.BatchID = b.BatchID and s.SubscriberID = b.SubscriberID and b.IsUnique = 1
LEFT JOIN _Unsubscribe u ON s.JobID = u.JobID and s.ListID = u.ListID and s.BatchID = u.BatchID and s.SubscriberID = u.SubscriberID and u.IsUnique = 1
LEFT JOIN _JourneyActivity ja ON j.TriggererSendDefinitionObjectID = ja.JourneyActivityObjectID
LEFT JOIN _Journey jou ON ja.VersionID = jou.VersionID
WHERE jou.JourneyName = 'Test_UK' AND jou.JourneyStatus = 'Running'
GROUP BY jou.JourneyName, jou.VersionNumber, jou.JourneyStatus, j.EmailName
