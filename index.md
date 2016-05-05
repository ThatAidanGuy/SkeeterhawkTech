<!DOCTYPE html>
  <html lang= en>
  <html>
  <head>
  <title>ScheduleLife</title>
  <link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-datepicker/1.5.1/css/bootstrap-datepicker3.min.css">

  <style>
  h1 {
    text-align: center;
  }
  .page {
    margin: 45px;


  }
  table.table {
    width: 100%
  }

  td {
     max-width: 25%;
  }

  td.attending {
  background-color: green
  }
  .table-buttons {
    margin-bottom: 15px;
  }
  .cf:before,
.cf:after {
    content: " "; /* 1 */
    display: table; /* 2 */
}

.cf:after {
    clear: both;
}
  .date-select-button {
    float: left;
    width: 150px;
  }
  .add-buttons {
    float: right;
  }
.remove-friend,
.remove-event {
  padding-left: 5px;
  position: relative;
  top: 2px;
  cursor: pointer;
  color: #a94442;
}

  </style>
   <script src="https://code.jquery.com/jquery-1.11.3.min.js"></script>
   <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
   <script src="bootstrap-datepicker.js"></script>
  </head>
  <body>
    <div class="page">
    <h1>ScheduleLife</h1>
    <div class="table-buttons cf">
      <div class="add-buttons">
      <button type="button" class="btn btn-default btn-primary friend-modal-button" data-toggle="modal" data-target="#friendModal">Add Friend</button>
      <button type="button" class="btn btn-default btn-primary event-modal-button" data-toggle="modal" data-target="#myModal">Add Event</button>
    </div>
      <div class="input-group date date-select-button">
          <input type="text" class="form-control">
          <div class="input-group-addon">
              <span class="glyphicon glyphicon-th"></span>
          </div>
      </div>

  </div>
    <div class="table-container"></div>
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
          <h4 class="modal-title" id="myModalLabel">Add Event</h4>
        </div>
        <div class="modal-body">
          <form>
            <div class="form-group">
              <label for="eventname ">Event Name</label>
              <input type="text" class="form-control" id="eventname" placeholder="Event name">
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary add-event-button">Add Event</button>
        </div>
      </div>
    </div>
    </div>


    <div class="modal fade" id="friendModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
          <h4 class="modal-title" id="myModalLabel">Add Friend</h4>
        </div>
        <div class="modal-body">
          <form>
            <div class="form-group">
              <label for="friendname ">Friend Name</label>
              <input type="text" class="form-control" id="friendname" placeholder="Friend name">
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary add-friend-button">Add Friend</button>
        </div>
      </div>
    </div>
    </div>
  </div>


  <div class="modal fade" id="removeFriend" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog bs-example-modal-sm" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Remove Friend</h4>
      </div>
      <div class="modal-body">
        <p>Are you sure you want to remove this friend?</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary remove-friend-modal-button">Remove Friend</button>
      </div>
    </div>
  </div>
  </div>
</div>

<div class="modal fade" id="removeEvent" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
<div class="modal-dialog" role="document">
  <div class="modal-content">
    <div class="modal-header">
      <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
      <h4 class="modal-title" id="myModalLabel">Remove Event</h4>
    </div>
    <div class="modal-body">
      <p>Are you sure you want to remove this event?</p>
    </div>
    <div class="modal-footer">
      <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      <button type="button" class="btn btn-primary remove-event-modal-button">Remove Event</button>
    </div>
  </div>
</div>
</div>
</div>

   <script>
   $( document ).ready(function() {
     var selectedDate = new Date().toDateString();
     var friends = [];
     var events = [];

     if (localStorage.getItem('scheduleLifeEvents')) {
       events = JSON.parse(localStorage.getItem('scheduleLifeEvents'))
     }

    if (localStorage.getItem('scheduleLifeFriends')) {
      friends = JSON.parse(localStorage.getItem('scheduleLifeFriends'))
    }

     function getEventsByDate (eventsArray, eventDate) {
       var filteredEvents = [];
       for (var x = 0; x < eventsArray.length; x++){
        var event = eventsArray[x];
        if (event.date === eventDate) {
          filteredEvents.push(event)
        }
       }
       return filteredEvents;
     }

     function getItemByID (arr, id) {
       for (var x = 0; x < arr.length; x++){
        if (arr[x].id === id) {
          return {ind: x, item: arr[x]}
        }
       }
     }

     function drawTableOrMessage() {
       var filteredEvents = getEventsByDate(events, selectedDate);
        if (filteredEvents.length === 0) {
          $msg = $('<div class="alert alert-info" role="alert"><p>There are no events for this date. Add an event to get started!</p> </div>')
          $('.table-container').html($msg)
        } else {
          drawTable(filteredEvents)
        }

     }

    function drawTable (filteredEvents) {
       var $table = $('<table class="table table-bordered events-table"></table>')

       var $userRow = $('<tr class="user-row"><td>You</td></tr>');
       for (var x = 0; x < filteredEvents.length; x++){
         var cellHTML = '<td class="time">' +
                          filteredEvents[x].title +
                          '<span class="glyphicon glyphicon-remove-circle remove-event" data-event="' + filteredEvents[x].id + '">' +
                          '</span>' +
                        '</td>';
         var $cell = $(cellHTML);
         $userRow.append($cell)
       }

       $table.append($userRow)

       for (var i = 0; i < friends.length; i++) {
         var friendName = friends[i].name;
         var friendID = friends[i].id;
         var $friendRow = $('<tr class="friend-row">' +
                              '<td>' +
                                friendName +
                                '<span class="glyphicon glyphicon-remove-circle remove-friend" data-friend="' + friendID + '">' +
                                '</span>' +
                              '</td>' +
                            '</tr>');

         for (var x = 0; x < filteredEvents.length; x++){
           var event = filteredEvents[x];
           var cellClass = "time";
           var attendees = event.attendees || [];
           if (attendees.indexOf(friendID) > -1) {
             cellClass += " attending";
           }
           var cellHTML = '<td class="' + cellClass + '" data-friend=' + friendID + ' data-event='+ event.id +'></td>';
           $friendRow.append($(cellHTML))
         }
         $table.append($friendRow)
       }
      $('.table-container').html($table)
    }


     $(".table-container").on("click", "tr.friend-row td.time", function (e) {
       var thingThatWasClicked = $(e.target);

       var friendID = thingThatWasClicked.data('friend')
       var eventID = thingThatWasClicked.data('event');
       var event = getItemByID(events, eventID).item;

       if (!event.attendees) {
         event.attendees = [];
       }

       if (thingThatWasClicked.hasClass("attending")) {
         thingThatWasClicked.removeClass("attending")
         var attendee = event.attendees.indexOf(friendID);
         event.attendees.splice(attendee, 1)
       }else {
         thingThatWasClicked.addClass("attending");
         if (event.attendees.indexOf(friendID) < 0) {
           event.attendees.push(friendID)
         }
       }

       localStorage.setItem('scheduleLifeEvents', JSON.stringify(events))
     })

     $(".add-event-button").on("click", function () {
       // get event data
       var eventName = $('#myModal #eventname').val();
       // hack to get a unique id
       var id = new Date().getTime();
       events.push({id: id, title: eventName, date:selectedDate});
       drawTableOrMessage();
       localStorage.setItem('scheduleLifeEvents', JSON.stringify(events))
       $('#myModal #eventname').val('');
       $('#myModal').modal('hide')
     });

     $(".add-friend-button").on("click", function () {
       // get friend data
       var friendNameInput = $('#friendModal #friendname');
       var friendName = friendNameInput.val();
       var id = new Date().getTime();
       friends.push({id: id, name: friendName});
       drawTableOrMessage();
       localStorage.setItem('scheduleLifeFriends', JSON.stringify(friends))
       friendNameInput.val('');
       $('#friendModal').modal('hide')
     });

     $(".table-container").on('click', '.remove-event', function (e) {
       var eventID = $(e.target).data('event');
       var obj =  getItemByID(events, eventID);
       var event = obj.item;
       var eventIndex = obj.ind;
       var eventName = event.title;
       $('#removeEvent')
          .find('.modal-body')
          .html('<p>Are you sure you want to remove <em>' + eventName + '</em>?</p>');
       $('#removeEvent').modal('show');

       $(".remove-event-modal-button").on("click", function () {
         events.splice(eventIndex, 1)
         drawTableOrMessage();
         localStorage.setItem('scheduleLifeEvents', JSON.stringify(events))
         $('#removeEvent').modal('hide')
       });
     });

     $(".table-container").on('click', '.remove-friend', function (e) {
       var friendID = $(e.target).data('friend');
       var obj =  getItemByID(friends, friendID);
       var friend = obj.item;
       var friendIndex = obj.ind;
       var friendName = friend.name;
       $('#removeFriend')
          .find('.modal-body')
          .html('<p>Are you sure you want to remove <em>' + friendName + '</em>?</p>');
       $('#removeFriend').modal('show');

       $(".remove-friend-modal-button").on("click", function () {
         friends.splice(friendIndex, 1);
         drawTableOrMessage();
         removeFriendFromEvents(friendID);
         localStorage.setItem('scheduleLifeFriends', JSON.stringify(friends))
         localStorage.setItem('scheduleLifeEvents', JSON.stringify(events))
         $('#removeFriend').modal('hide')
         console.log(events)
       });
     });

     function removeFriendFromEvents (friendID) {
       for (var e = 0; e < events.length; e++) {
           var attendees = events[e].attendees || [];
           for (var i = 0; i < attendees.length; i++) {
             if (attendees[i] == friendID) {
               attendees.splice(i, 1);
             }
           }
       }
     }


     $('.date-select-button').datepicker(
       {
         autoclose: true,
         clearBtn: true,
         todayHighlight: true
       }
     ).on('changeDate', function(e) {
        selectedDate = new Date(e.date).toDateString();
        drawTableOrMessage();
     });

     $('.date-select-button').datepicker('setDate', selectedDate)
   });
   </script>
 </body>
 </html>
