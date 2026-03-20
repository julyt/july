import React, { useMemo, useState } from "react";
import { Card, CardContent, CardHeader, CardTitle, CardDescription } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { Badge } from "@/components/ui/badge";
import { Checkbox } from "@/components/ui/checkbox";
import { Textarea } from "@/components/ui/textarea";
import { Tabs, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { motion } from "framer-motion";
import { Plus, Search, Trash2, Users, CheckCircle2, Clock3, AlertCircle, Vote, Megaphone, Phone, DoorOpen } from "lucide-react";

const starterTasks = [
  {
    id: 1,
    title: "Finalize weekend canvass turf",
    assignee: "Field",
    team: "Field",
    priority: "High",
    status: "In Progress",
    due: "2026-03-22",
    notes: "Need walk packets for District 2 and volunteer captain confirmation.",
    blocked: false,
  },
  {
    id: 2,
    title: "Draft fundraising follow-up email",
    assignee: "Comms",
    team: "Finance",
    priority: "Medium",
    status: "To Do",
    due: "2026-03-23",
    notes: "Segment donors who gave in the last 60 days.",
    blocked: false,
  },
  {
    id: 3,
    title: "QA volunteer sign-up form",
    assignee: "July",
    team: "Tech",
    priority: "High",
    status: "Blocked",
    due: "2026-03-21",
    notes: "Need mobile spacing fix and duplicate submission check.",
    blocked: true,
  },
  {
    id: 4,
    title: "Schedule endorsement graphics",
    assignee: "Design",
    team: "Comms",
    priority: "Low",
    status: "Done",
    due: "2026-03-20",
    notes: "Queue for Instagram + X + email header.",
    blocked: false,
  },
];

const teamIcons = {
  Field: DoorOpen,
  Finance: Vote,
  Comms: Megaphone,
  Tech: Phone,
};

const priorities = ["Low", "Medium", "High"];
const statuses = ["To Do", "In Progress", "Blocked", "Done"];
const teams = ["Field", "Finance", "Comms", "Tech"];

function StatCard({ label, value, icon: Icon, sublabel }) {
  return (
    <Card className="rounded-2xl shadow-sm">
      <CardContent className="p-4">
        <div className="flex items-start justify-between gap-3">
          <div>
            <p className="text-sm text-slate-500">{label}</p>
            <p className="mt-1 text-2xl font-semibold tracking-tight">{value}</p>
            {sublabel ? <p className="mt-1 text-xs text-slate-500">{sublabel}</p> : null}
          </div>
          <div className="rounded-2xl bg-slate-100 p-2">
            <Icon className="h-5 w-5" />
          </div>
        </div>
      </CardContent>
    </Card>
  );
}

function PriorityBadge({ priority }) {
  const styles = {
    Low: "bg-slate-100 text-slate-700",
    Medium: "bg-amber-100 text-amber-800",
    High: "bg-rose-100 text-rose-800",
  };

  return <Badge className={`${styles[priority]} border-0`}>{priority}</Badge>;
}

function StatusBadge({ status }) {
  const styles = {
    "To Do": "bg-slate-100 text-slate-700",
    "In Progress": "bg-blue-100 text-blue-800",
    Blocked: "bg-red-100 text-red-800",
    Done: "bg-emerald-100 text-emerald-800",
  };

  return <Badge className={`${styles[status]} border-0`}>{status}</Badge>;
}

export default function CampaignTaskTrackerPrototype() {
  const [tasks, setTasks] = useState(starterTasks);
  const [search, setSearch] = useState("");
  const [teamFilter, setTeamFilter] = useState("All");
  const [statusFilter, setStatusFilter] = useState("All");
  const [view, setView] = useState("All");
  const [form, setForm] = useState({
    title: "",
    assignee: "",
    team: "Field",
    priority: "Medium",
    status: "To Do",
    due: "",
    notes: "",
    blocked: false,
  });

  const filteredTasks = useMemo(() => {
    return tasks.filter((task) => {
      const matchesSearch = [task.title, task.assignee, task.team, task.notes]
        .join(" ")
        .toLowerCase()
        .includes(search.toLowerCase());

      const matchesTeam = teamFilter === "All" || task.team === teamFilter;
      const matchesStatus = statusFilter === "All" || task.status === statusFilter;
      const matchesView = view === "All" || task.status === view;

      return matchesSearch && matchesTeam && matchesStatus && matchesView;
    });
  }, [tasks, search, teamFilter, statusFilter, view]);

  const stats = useMemo(() => {
    return {
      total: tasks.length,
      inProgress: tasks.filter((t) => t.status === "In Progress").length,
      blocked: tasks.filter((t) => t.status === "Blocked").length,
      done: tasks.filter((t) => t.status === "Done").length,
    };
  }, [tasks]);

  function addTask() {
    if (!form.title.trim()) return;

    const newTask = {
      ...form,
      id: Date.now(),
      blocked: form.status === "Blocked" ? true : form.blocked,
    };

    setTasks((prev) => [newTask, ...prev]);
    setForm({
      title: "",
      assignee: "",
      team: "Field",
      priority: "Medium",
      status: "To Do",
      due: "",
      notes: "",
      blocked: false,
    });
  }

  function updateTask(id, field, value) {
    setTasks((prev) =>
      prev.map((task) =>
        task.id === id
          ? {
              ...task,
              [field]: value,
              blocked: field === "status" ? value === "Blocked" : task.blocked,
            }
          : task
      )
    );
  }

  function toggleDone(id, checked) {
    setTasks((prev) =>
      prev.map((task) =>
        task.id === id
          ? {
              ...task,
              status: checked ? "Done" : "To Do",
              blocked: false,
            }
          : task
      )
    );
  }

  function deleteTask(id) {
    setTasks((prev) => prev.filter((task) => task.id !== id));
  }

  return (
    <div className="min-h-screen bg-slate-50 p-4 md:p-8">
      <div className="mx-auto max-w-7xl space-y-6">
        <motion.div
          initial={{ opacity: 0, y: 12 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.35 }}
          className="grid gap-4 lg:grid-cols-[1.4fr_.9fr]"
        >
          <Card className="rounded-3xl border-0 shadow-sm">
            <CardHeader className="space-y-3 p-6">
              <div className="flex items-start justify-between gap-4">
                <div>
                  <CardTitle className="text-3xl tracking-tight">Campaign Task Tracker</CardTitle>
                  <CardDescription className="mt-2 max-w-2xl text-sm leading-6 text-slate-600">
                    A lightweight prototype for managing campaign work across field, comms, finance, and tech. Built to demonstrate product thinking, fast iteration, and clean UI for messy real-world coordination.
                  </CardDescription>
                </div>
                <div className="hidden rounded-3xl bg-slate-100 p-3 md:block">
                  <Vote className="h-8 w-8" />
                </div>
              </div>
              <div className="flex flex-wrap gap-2 text-xs text-slate-600">
                <Badge variant="outline" className="rounded-full">React prototype</Badge>
                <Badge variant="outline" className="rounded-full">Product-focused</Badge>
                <Badge variant="outline" className="rounded-full">Campaign ops</Badge>
              </div>
            </CardHeader>
          </Card>

          <Card className="rounded-3xl border-0 shadow-sm">
            <CardHeader className="p-6 pb-2">
              <CardTitle className="text-lg">Quick Add</CardTitle>
              <CardDescription>Add a task like a real campaign team would.</CardDescription>
            </CardHeader>
            <CardContent className="space-y-3 p-6 pt-2">
              <Input
                placeholder="Task title"
                value={form.title}
                onChange={(e) => setForm((prev) => ({ ...prev, title: e.target.value }))}
              />
              <div className="grid gap-3 sm:grid-cols-2">
                <Input
                  placeholder="Assignee"
                  value={form.assignee}
                  onChange={(e) => setForm((prev) => ({ ...prev, assignee: e.target.value }))}
                />
                <Input
                  type="date"
                  value={form.due}
                  onChange={(e) => setForm((prev) => ({ ...prev, due: e.target.value }))}
                />
              </div>
              <div className="grid gap-3 sm:grid-cols-3">
                <Select value={form.team} onValueChange={(value) => setForm((prev) => ({ ...prev, team: value }))}>
                  <SelectTrigger><SelectValue placeholder="Team" /></SelectTrigger>
                  <SelectContent>
                    {teams.map((team) => <SelectItem key={team} value={team}>{team}</SelectItem>)}
                  </SelectContent>
                </Select>
                <Select value={form.priority} onValueChange={(value) => setForm((prev) => ({ ...prev, priority: value }))}>
                  <SelectTrigger><SelectValue placeholder="Priority" /></SelectTrigger>
                  <SelectContent>
                    {priorities.map((priority) => <SelectItem key={priority} value={priority}>{priority}</SelectItem>)}
                  </SelectContent>
                </Select>
                <Select value={form.status} onValueChange={(value) => setForm((prev) => ({ ...prev, status: value }))}>
                  <SelectTrigger><SelectValue placeholder="Status" /></SelectTrigger>
                  <SelectContent>
                    {statuses.map((status) => <SelectItem key={status} value={status}>{status}</SelectItem>)}
                  </SelectContent>
                </Select>
              </div>
              <Textarea
                placeholder="Notes"
                value={form.notes}
                onChange={(e) => setForm((prev) => ({ ...prev, notes: e.target.value }))}
                className="min-h-20"
              />
              <Button onClick={addTask} className="w-full rounded-2xl">
                <Plus className="mr-2 h-4 w-4" />
                Add Task
              </Button>
            </CardContent>
          </Card>
        </motion.div>

        <div className="grid gap-4 md:grid-cols-2 xl:grid-cols-4">
          <StatCard label="Total Tasks" value={stats.total} icon={Users} sublabel="Across all campaign teams" />
          <StatCard label="In Progress" value={stats.inProgress} icon={Clock3} sublabel="Currently moving" />
          <StatCard label="Blocked" value={stats.blocked} icon={AlertCircle} sublabel="Needs a decision or fix" />
          <StatCard label="Done" value={stats.done} icon={CheckCircle2} sublabel="Completed work" />
        </div>

        <Card className="rounded-3xl border-0 shadow-sm">
          <CardContent className="space-y-4 p-6">
            <div className="flex flex-col gap-3 lg:flex-row lg:items-center lg:justify-between">
              <Tabs value={view} onValueChange={setView}>
                <TabsList className="grid w-full grid-cols-5 lg:w-auto">
                  <TabsTrigger value="All">All</TabsTrigger>
                  <TabsTrigger value="To Do">To Do</TabsTrigger>
                  <TabsTrigger value="In Progress">In Progress</TabsTrigger>
                  <TabsTrigger value="Blocked">Blocked</TabsTrigger>
                  <TabsTrigger value="Done">Done</TabsTrigger>
                </TabsList>
              </Tabs>
              <div className="grid gap-3 md:grid-cols-[1.3fr_.8fr_.8fr]">
                <div className="relative">
                  <Search className="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-slate-400" />
                  <Input value={search} onChange={(e) => setSearch(e.target.value)} placeholder="Search tasks, assignees, notes" className="pl-9" />
                </div>
                <Select value={teamFilter} onValueChange={setTeamFilter}>
                  <SelectTrigger><SelectValue placeholder="Team" /></SelectTrigger>
                  <SelectContent>
                    <SelectItem value="All">All teams</SelectItem>
                    {teams.map((team) => <SelectItem key={team} value={team}>{team}</SelectItem>)}
                  </SelectContent>
                </Select>
                <Select value={statusFilter} onValueChange={setStatusFilter}>
                  <SelectTrigger><SelectValue placeholder="Status" /></SelectTrigger>
                  <SelectContent>
                    <SelectItem value="All">All statuses</SelectItem>
                    {statuses.map((status) => <SelectItem key={status} value={status}>{status}</SelectItem>)}
                  </SelectContent>
                </Select>
              </div>
            </div>

            <div className="grid gap-4">
              {filteredTasks.map((task, index) => {
                const TeamIcon = teamIcons[task.team] || Vote;
                return (
                  <motion.div
                    key={task.id}
                    initial={{ opacity: 0, y: 8 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ duration: 0.2, delay: index * 0.03 }}
                  >
                    <Card className="rounded-3xl border border-slate-200 shadow-none">
                      <CardContent className="p-5">
                        <div className="flex flex-col gap-4 xl:flex-row xl:items-start xl:justify-between">
                          <div className="space-y-3">
                            <div className="flex flex-wrap items-center gap-2">
                              <div className="rounded-2xl bg-slate-100 p-2"><TeamIcon className="h-4 w-4" /></div>
                              <h3 className="text-lg font-semibold tracking-tight">{task.title}</h3>
                              <PriorityBadge priority={task.priority} />
                              <StatusBadge status={task.status} />
                            </div>
                            <div className="flex flex-wrap gap-x-5 gap-y-1 text-sm text-slate-600">
                              <span><strong>Assignee:</strong> {task.assignee || "Unassigned"}</span>
                              <span><strong>Team:</strong> {task.team}</span>
                              <span><strong>Due:</strong> {task.due || "No date"}</span>
                            </div>
                            {task.notes ? <p className="max-w-3xl text-sm leading-6 text-slate-600">{task.notes}</p> : null}
                          </div>

                          <div className="grid gap-3 sm:grid-cols-2 xl:w-[370px]">
                            <Select value={task.status} onValueChange={(value) => updateTask(task.id, "status", value)}>
                              <SelectTrigger><SelectValue /></SelectTrigger>
                              <SelectContent>
                                {statuses.map((status) => <SelectItem key={status} value={status}>{status}</SelectItem>)}
                              </SelectContent>
                            </Select>
                            <Select value={task.priority} onValueChange={(value) => updateTask(task.id, "priority", value)}>
                              <SelectTrigger><SelectValue /></SelectTrigger>
                              <SelectContent>
                                {priorities.map((priority) => <SelectItem key={priority} value={priority}>{priority}</SelectItem>)}
                              </SelectContent>
                            </Select>
                            <div className="flex items-center gap-2 rounded-2xl border px-3">
                              <Checkbox
                                checked={task.status === "Done"}
                                onCheckedChange={(checked) => toggleDone(task.id, Boolean(checked))}
                                id={`done-${task.id}`}
                              />
                              <label htmlFor={`done-${task.id}`} className="text-sm text-slate-700">Mark complete</label>
                            </div>
                            <Button variant="outline" onClick={() => deleteTask(task.id)} className="rounded-2xl">
                              <Trash2 className="mr-2 h-4 w-4" />
                              Delete
                            </Button>
                          </div>
                        </div>
                      </CardContent>
                    </Card>
                  </motion.div>
                );
              })}

              {filteredTasks.length === 0 ? (
                <Card className="rounded-3xl border-dashed shadow-none">
                  <CardContent className="p-10 text-center text-sm text-slate-500">
                    No tasks match these filters yet.
                  </CardContent>
                </Card>
              ) : null}
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
