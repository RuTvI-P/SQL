# Data Analysis

# Overview 
Many insurance claims require regular publishes on the Reserving Tool, so we need to develop a procedure that will:
1. Identify which claims require regular publishes on the Reserving Tool.
2. Calculate how many days left the current examiner has to complete a publish, or if the
publish is late, how many days a publish is overdue.
3. Display key information related to these claims including the calculated fields in step 2.
4. Accept parameters allowing users to see results for specific situations.
5. The final query should filter out the claims that don’t require a publish, and should display the
required fields that will give us the information requested for this project.

# Parameters 

- Days To Complete
    - Displays claims with less than or equal to the inputted number of days to
    complete a publish
- Days Overdue
    - Displays claims with greater than or equal to the inputted number of days that a
    publish is overdue
- Office
    - Displays claims with examiners based in the inputted office
- Manager Code
    - Displays claims with claims managed by users with the inputted manager code
- Supervisor Code
    - Displays claims supervised by users with the inputted supervisor code
- Examiner Code
    - Displays claims examined by users with the inputted examiner code
- Team
    - Displays claims where at least one of the examiner’s, supervisor’s, or manager’s
    titles includes the inputted string (e.g. if the word “analyst” is inputted, one or
    more titles should include the word “analyst”)
- Claims Without a Publish on the Reserving Tool
    - Displays claims that do not yet have a publish on a Reserving Tool if a “1” is
    inputted.

# Details 
1. **Identifying which claims require regular publishes on the Reserving Tool**
only includes claims where:
    - The claim status is currently open, or it is currently re-opened and the re-open reason is
    not to pay a bill (i.e. ReopenReasonID != 3)
    - The office that the claim is being handled is in Sacramento, San Francisco, or San Diego
    - At least one of the following is true:
        - The claimant type is “First Aid” or “Medical-Only”
        - The office is in San Diego, the examiner’s title includes “analyst”, and the total
        reserve sum of the five reserve type buckets (Medical, TD, PD, Rehab, and
        Expense.. I.e. all buckets except Fatality) is greater than the examiner’s reserve
        limit
        - The office is in either Sacramento or San Francisco, and one of the following is
        true:
            - The total reserve amount in the medical reserve bucket for the claim is
            greater than $1000
            - The total reserve amount in the expense reserve bucket for the claim is
            greater than $100
            - The sum of the total reserve amount in the TD, PD, and Rehab reserve
            buckets for the claim is greater than $0
2. **Calculate days left/days overdue for a publish**
examiners are required to complete a publish in the reserving tool every so often. The rules as
to when they need to complete a publish are as follows:
    - If there are no publishes on the claim, the examiner has 14 days from when they were
    assigned the claim to complete a publish
    - If there is at least one publish on the claim, the examiner has the later of 90 days after
    the last publish or 14 days after they were assigned the claim
    Days To Complete - if the examiner still has time to complete a publish before it is due, then this
    field will be the number of days left. Otherwise, if a publish is overdue, this field should be 0.

Days To Complete - if the examiner still has time to complete a publish before it is due, then this
field will be the number of days left. Otherwise, if a publish is overdue, this field should be 0.

Days Overdue - if the examiner still has time to complete a publish before it is due, then this
field should be 0. Otherwise, if a publish is overdue, this field should be the number of days that
the publish is overdue.
When calculating how many days until the examiner needs to complete a publish, calculate the
dates as of January 1, 2019.

# Results

The results of the query should display the following fields for each applicable claim [brackets is
the table the field can be found in]:
- Claim Number [Claim]
- Examiner Code, Name, and Title [Users]
- Supervisor Code, Name, and Title [Users]
- Manager (Supervisor of the Supervisor) Code, Name, and Title [Users]
- Office [Office]
- Claim Status [ClaimStatus]
- Claimant Name [Patient]
- Claimant Type [ClaimantType]
- Current Examiner Assigned Date [ClaimLog]
- Reopened Date [Claimant]
- Adjusted Assigned Date = (later of Assigned Date and Reopened Date)
- Last Published Date [ReservingTool]
- Days Since Last Published Date 
- Days Since Adjusted Assigned Date 
- Days To Complete 
- Days Overdue 
